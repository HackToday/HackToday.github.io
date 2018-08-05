title: 'Openstack keystone的policy设计处理分析'
date: 2014-07-31 14:45:08
tags: 云计算
---

keystone设计的policy role based access controls(RBAC)非常有意思，最终暴露给用户的是一种简单的可编辑的语义规则。
举个例子如下：

```
{   
  ... 
 "admin_required": "role:admin or is_admin:1",
 "identity:get_user": "rule:admin_required",
 ...
}
```

keystone的policy使用的driver默认配置是如下：
driver = keystone.policy.backends.sql.Policy


每个相应的API的访问请求都会经过相应RBAC的检测，
在keystone中的API是通过filterprotected，protected的decorator来作用的，
调用入口：

```
                    self.policy_api.enforce(creds,
                                        action,
                                        authorization.flatten(policy_dict))
```

具体的时序图以get_user为例如下：


Enforcer这个类其中的enforce方法逻辑如下：

```
        ....
        self.load_rules()

        # Allow the rule to be a Check tree
        if isinstance(rule, BaseCheck):
            result = rule(target, creds, self)
        elif not self.rules:
            # No rules to reference means we're going to fail closed
            result = False
        else:
            try:
                # Evaluate the rule
                result = self.rules[rule](target, creds, self)
            except KeyError:
                LOG.debug(_("Rule [%s] doesn't exist") % rule)
                # If the rule doesn't exist, fail closed
                result = False
       ...
```

其中各种rule的就是各种规则来实现不同角色的权限管理，
具体的rule类层次结构如下：


下次我们将对其policy语言的解析处理进行详细的分析
