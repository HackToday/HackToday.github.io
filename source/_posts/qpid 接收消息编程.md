title: 'qpid 接收消息编程'
date: 2013-10-18 17:06:25
tags: Python/Ruby
---

qpid 是符合AMQP规范的apache 许可证的消息中间件，目前在openstack中作为一种可选的消息中间件服务配置，其他还有rabbitmq和zeroMQ

如果看过qpid的编程API文档的话，会看到比较简单的一个例子， 如下（接收消息打印消息内容）

```
#following code is from
# http://qpid.apache.org/releases/qpid-0.24/messaging-api/python/examples/drain.html


import optparse
from qpid.messaging import *
from qpid.util import URL
from qpid.log import enable, DEBUG, WARN

parser = optparse.OptionParser(usage="usage: %prog [options] ADDRESS ...",
                               description="Drain messages from the supplied address.")
parser.add_option("-b", "--broker", default="localhost",
                  help="connect to specified BROKER (default %default)")
parser.add_option("-c", "--count", type="int",
                  help="number of messages to drain")
parser.add_option("-f", "--forever", action="store_true",
                  help="ignore timeout and wait forever")
parser.add_option("-r", "--reconnect", action="store_true",
                  help="enable auto reconnect")
parser.add_option("-i", "--reconnect-interval", type="float", default=3,
                  help="interval between reconnect attempts")
parser.add_option("-l", "--reconnect-limit", type="int",
                  help="maximum number of reconnect attempts")
parser.add_option("-t", "--timeout", type="float", default=0,
                  help="timeout in seconds to wait before exiting (default %default)")
parser.add_option("-p", "--print", dest="format", default="%(M)s",
                  help="format string for printing messages (default %default)")
parser.add_option("-v", dest="verbose", action="store_true",
                  help="enable logging")

opts, args = parser.parse_args()

if opts.verbose:
  enable("qpid", DEBUG)
else:
  enable("qpid", WARN)

if args:
  addr = args.pop(0)
else:
  parser.error("address is required")
if opts.forever:
  timeout = None
else:
  timeout = opts.timeout

class Formatter:

  def __init__(self, message):
    self.message = message
    self.environ = {"M": self.message,
                    "P": self.message.properties,
                    "C": self.message.content}

  def __getitem__(self, st):
    return eval(st, self.environ)

conn = Connection(opts.broker,
                  reconnect=opts.reconnect,
                  reconnect_interval=opts.reconnect_interval,
                  reconnect_limit=opts.reconnect_limit)
try:
  conn.open()
  ssn = conn.session()
  rcv = ssn.receiver(addr)

  count = 0
  while not opts.count or count < opts.count:
    try:
      msg = rcv.fetch(timeout=timeout)
      print opts.format % Formatter(msg)
      count += 1
      ssn.acknowledge()
    except Empty:
      break
except ReceiverError, e:
  print e
except KeyboardInterrupt:
  pass

conn.close()
```

connection和session是1对多的关系，每个session保证消息的顺序接收，让session创建对应的sender和receiver，这里我们只需要创建receiver

这个程序看起来很好，如果直接传一个地址， 比如（我们想要接收openstack glance的message）
运行如下：
python drain.py -b admin/qpid@localhost glance

如果我们想要实现连接断掉后重新自动连接，我们可以传入参数 -r
上面的程序有个问题，如果没有收到消息就断开连接了。

新需求1： 我们要实现持续的监听接收消息， 改一下

```
try:
      msg = rcv.fetch(timeout=timeout)
      print opts.format % Formatter(msg)
      count += 1
      ssn.acknowledge()
except Empty:
      time.sleep(0.5)
```

这样就可以了。

还不行，

新需求2：我们要实现断后重新自动连接， 可以， 传入参数 -r，

解决了

问题出现了，你会发现在service qpidd restart后，程序会抛出exception
qpid.messaging.exceptions.NotFound: no such queue: glance

新需求3：显然这是由于我们创建的queue不是durable的，所以需要在qpid restart后能够正常运行，而不是退出。
这就需要我们创建的queue能够在接收到消息的时候自动创建完成，

解决方法：     rcv = ssn.receiver(addr + "; {create: always}")

这样就完成queue的按需创建


新需求4： qpid 默认的reconnect上面的是基于固定interval，我们想改变重新建立连接的算法，实现2的指数式建立连接
解决方法： 我们写入自己的自动重连方法， 一个简单的例子如下：

```
def reconnect():
    global rcv
    global ssn
    attempt = 0
    delay = 1
    while True:
        if conn.opened():
            try:
                conn.close()
            except exceptions.ConnectionError:
                    pass
        attempt += 1
        print "The %s time attempt for reconnecting qpid server" % str(attempt)
        try:
            connection_init()
            conn.open()
        except exceptions.ConnectionError, e:
            delay = min(2 * delay, 60)
            time.sleep(delay)
            pass
        else:
            break
    print "qpid server reconnection created"
    ssn = conn.session()
    rcv = ssn.receiver(addr + "; {create: always}")
```

这样就OK了

总结： 经过上面的需求变化，我们新的接收消息程序如下：

```
import optparse
import time
from qpid.messaging import *
from qpid.util import URL
from qpid.log import enable, DEBUG, WARN

parser = optparse.OptionParser(usage="usage: %prog [options] ADDRESS ...",
                               description="Drain messages from the supplied address.")
parser.add_option("-b", "--broker", default="localhost",
                  help="connect to specified BROKER (default %default)")
parser.add_option("-c", "--count", type="int",
                  help="number of messages to drain")
parser.add_option("-f", "--forever", action="store_true",
                  help="ignore timeout and wait forever")
parser.add_option("-r", "--reconnect", action="store_true",
                  help="enable auto reconnect")
parser.add_option("-i", "--reconnect-interval", type="float", default=3,
                  help="interval between reconnect attempts")
parser.add_option("-l", "--reconnect-limit", type="int",
                  help="maximum number of reconnect attempts")
parser.add_option("-t", "--timeout", type="float", default=0,
                  help="timeout in seconds to wait before exiting (default %default)")
parser.add_option("-p", "--print", dest="format", default="%(M)s",
                  help="format string for printing messages (default %default)")
parser.add_option("-v", dest="verbose", action="store_true",
                  help="enable logging")

opts, args = parser.parse_args()

if opts.verbose:
    enable("qpid", DEBUG)
else:
    enable("qpid", WARN)

if args:
    addr = args.pop(0)
else:
    parser.error("address is required")
if opts.forever:
    timeout = None
else:
    timeout = opts.timeout

count = 0
rcv = None
conn = None
ssn = None


class Formatter:

    def __init__(self, message):
        self.message = message
        self.environ = {"M": self.message,
                    "P": self.message.properties,
                    "C": self.message.content}

    def __getitem__(self, st):
        return eval(st, self.environ)


def reconnect():
    global rcv
    global ssn
    attempt = 0
    delay = 1
    while True:
        if conn.opened():
            try:
                conn.close()
            except exceptions.ConnectionError:
                    pass
        attempt += 1
        print "The %s time attempt for reconnecting qpid server" % str(attempt)
        try:
            connection_init()
            conn.open()
        except exceptions.ConnectionError, e:
            delay = min(2 * delay, 60)
            time.sleep(delay)
            pass
        else:
            break
    print "qpid server reconnection created"
    ssn = conn.session()
    rcv = ssn.receiver(addr + "; {create: always}")


def fetch():
    global count
    while not opts.count or count < opts.count:
        try:
            msg = rcv.fetch(timeout=timeout)
            print opts.format % Formatter(msg)
            count += 1
            ssn.acknowledge()
        except Empty:
            time.sleep(0.5)
        except exceptions.ConnectionError, e:
            reconnect()
        except Exception, e:
            print e
            raise e


def connection_init():
    global conn
    conn = Connection(opts.broker,
                  reconnect=opts.reconnect,
                  reconnect_interval=opts.reconnect_interval,
                  reconnect_limit=opts.reconnect_limit)


try:
    connection_init()
    conn.open()
    ssn = conn.session()
    rcv = ssn.receiver(addr + "; {create: always}")
    fetch()

except ReceiverError, e:
    print e
except KeyboardInterrupt:
    pass
except exceptions.ConnectionError, e:
    reconnect()


conn.close()
```