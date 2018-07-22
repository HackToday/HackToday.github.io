title: qpid的地址解析
date: 2013-11-09 15:53:40
tags: Python/Ruby
---


						qpid的地址字符串按照一定语法进行匹配，确定相应的name subject和option

address := name [ SLASH subject ] [ ";" options ] 
   name := ( part | quoted )+
subject := ( part | quoted | SLASH )*
 quoted := STRING / ESC
   part := LBRACE / RBRACE / COLON / COMMA / NUMBER / ID / SYM
options := map
    map := "{" ( keyval ( "," keyval )* )? "}"
 keyval "= ID ":" value
  value := NUMBER / STRING / ID / map / list
   list := "[" ( value ( "," value )* )? "]"


其中相应的pattern如下：


LBRACE: \\{
RBRACE: \\}
LBRACK: \\[
RBRACK: \\]
COLON:  :
SEMI:   ;
SLASH:  /
COMMA:  ,
NUMBER: [+-]?[0-9]*\\.?[0-9]+
ID:     [a-zA-Z_](?:[a-zA-Z0-9_-]*[a-zA-Z0-9_])?
STRING: "(?:[^\\\\"]|\\\\.)*"|\'(?:[^\\\\\']|\\\\.)*\'
ESC:    \\\\[^ux]|\\\\x[0-9a-fA-F][0-9a-fA-F]|\\\\u[0-9a-fA-F][0-9a-fA-F][0-9a-fA-F][0-9a-fA-F]
SYM:    [.#*%@$^!+-]
WSPACE: [ \\n\\r\\t]+

在qpid messaging的API实现中，python版本是如下解析：
  def address(self):
   #获取name
    name = toks2str(self.eat_until(SLASH, SEMI, EOF))


    if name is None:
      raise ParseError(self.next())

    #获取subject
    if self.matches(SLASH):
      self.eat(SLASH)
      subject = toks2str(self.eat_until(SEMI, EOF))
    else:
      subject = None

    #获取option
    if self.matches(SEMI):
      self.eat(SEMI)
      options = self.map()
    else:
      options = None
    return name, subject, options


显然其中的分割符 SLASH 和SEMI进行了相应的划分
如果我们在qpid构造地址的字符串编程中传入了违法的字符，那么地址解析验证中就会报错


参考：
1. http://qpid.apache.org/releases/qpid-0.14/books/Programming-In-Apache-Qpid/html/ch02s04.html#table-node-properties 2. http://qpid.apache.org/components/messaging-api/index.html 
                                    