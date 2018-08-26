title: '故障分析平台 Sentry 的 LDAP 集成搭建'
date: 2017-12-21 21:57:33
tags: 云计算
---

sentry 是一个支持软件系统故障信息报告和聚集的平台，可以扩平台的部署和管理故障信息，不同于传统的 ELK 日志分析平台，sentry 更加专注对于故障和异常类信息的处理，减少无关消息的干扰，实时的汇集和报告软件系统的潜在问题，帮助开发和管理人员迅速的解决问题。

sentry 本身提供了扩展点，方便第三方来实现相关的系统集成，以 LDAP 为例，sentry-ldap-auth 是一个 django 定制化实现，支持 LDAP， 具体的配置在 github 介绍中比较简陋，所以用户在配置中容易出现各类奇怪的问题，所以，今天我们以一个系统为例，说说 LDAP 相关的配置。

1. 首先是 sentry 本身的安装，这个比较简单，

下载 https://github.com/getsentry/onpremise，然后进行 docker 镜像的构建，在构建之前，需要根据实际情况对 sentry.conf.py 进行配置，这里就涉及到 LDAP 插件的定制，

2. 对 LDAP 相关的插件进行定制，以 https://github.com/Banno/getsentry-ldap-auth 为文档基础，按照我们的 LDAP 基础设施情况配置，例如：

```
import ldap
from django_auth_ldap.config import LDAPSearch, GroupOfUniqueNamesType
AUTH_LDAP_SERVER_URI = 'xxxxx'
AUTH_LDAP_BIND_DN = 'xxxxx'
AUTH_LDAP_BIND_PASSWORD = 'xxxxx'
OU=unicode('xxxxx', 'utf8')
AUTH_LDAP_USER_SEARCH = LDAPSearch(
    OU,
    ldap.SCOPE_SUBTREE,
    '(sAMAccountName=%(user)s)',
)
AUTH_LDAP_GROUP_SEARCH = LDAPSearch(
    '',
    ldap.SCOPE_SUBTREE,
    '(objectClass=groupOfUniqueNames)'
)
AUTH_LDAP_USER_ATTR_MAP = {
    "username": 'xxxxx',
    "first_name": "xxxxx",
    "last_name": "xxxxx",
    "email": "xxxxx"
}
AUTH_LDAP_GROUP_TYPE = GroupOfUniqueNamesType()
AUTH_LDAP_REQUIRE_GROUP = None
AUTH_LDAP_DENY_GROUP = None
AUTH_LDAP_FIND_GROUP_PERMS = False
AUTH_LDAP_CACHE_GROUPS = True
AUTH_LDAP_GROUP_CACHE_TIMEOUT = 3600
AUTH_LDAP_DEFAULT_SENTRY_ORGANIZATION = u'xxxxx'
AUTH_LDAP_SENTRY_ORGANIZATION_ROLE_TYPE = 'member'
AUTH_LDAP_SENTRY_ORGANIZATION_GLOBAL_ACCESS = True
AUTHENTICATION_BACKENDS = (
    'sentry_ldap_auth.backend.SentryLdapBackend',
    'django.contrib.auth.backends.ModelBackend',
)
AUTH_LDAP_DEFAULT_EMAIL_DOMAIN='xxxxx'
SENTRY_EMAIL_BACKEND='dummy'
SENTRY_BEACON = False
```

3.执行 make build 构建 docker 镜像

4.根据构建好的镜像进行安装 https://docs.sentry.io/server/installation/docker/，如下：（这里没有安装 email 相关的邮件传输服务）

```
docker run \
--detach \
--name sentry-redis \
redis:3.2-alpine

docker run \
--detach \
--name sentry-postgres \
--env POSTGRES_PASSWORD=secret \
--env POSTGRES_USER=sentry \
postgres:9.5


docker run \
--rm sentry-onpremise \
config generate-secret-key

docker run \
--rm -it \
--link sentry-redis:redis \
--link sentry-postgres:postgres \
--env SENTRY_SECRET_KEY=${SENTRY_SECRET_KEY} \
sentry-onpremise upgrade


docker run \
-it \
--name sentry-web-01 \
--link sentry-redis:redis \
--link sentry-postgres:postgres \
--env SENTRY_SECRET_KEY=${SENTRY_SECRET_KEY} \
--publish 9000:9000 \
sentry-onpremise \
run web


docker run \
--detach \
--name sentry-worker-01 \
--link sentry-redis:redis \
--link sentry-postgres:postgres \
--env SENTRY_SECRET_KEY=${SENTRY_SECRET_KEY} \
sentry-onpremise \
run worker


docker run \
--detach \
--name sentry-cron \
--link sentry-redis:redis \
--link sentry-postgres:postgres \
--env SENTRY_SECRET_KEY=${SENTRY_SECRET_KEY} \
sentry-onpremise \
run cron
```

5.验证
访问 http://localhost:9000 然后使用 LDAP 账户访问就可以了

其他相关参考文献：
http://thefourtheye.in/2013/04/20/installing-python-ldap-in-ubuntu/
http://www.cnblogs.com/dreamer-fish/p/5474289.html