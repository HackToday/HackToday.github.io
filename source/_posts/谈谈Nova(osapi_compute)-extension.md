title: '谈谈Nova(osapi_compute)-extension'
date: 2013-07-04 22:57:55
tags: 云计算
---

```
[composite:osapi_compute]
use = call:nova.api.openstack.urlmap:urlmap_factory
/: oscomputeversions
/v1.1: openstack_compute_api_v2
/v2: openstack_compute_api_v2



[composite:openstack_compute_api_v2]
use = call:nova.api.auth:pipeline_factory
noauth = faultwrap sizelimit noauth ratelimit osapi_compute_app_v2
keystone = faultwrap sizelimit authtoken keystonecontext ratelimit osapi_compute_app_v2
keystone_nolimit = faultwrap sizelimit authtoken keystonecontext osapi_compute_app_v2


[app:osapi_compute_app_v2]
paste.app_factory = nova.api.openstack.compute:APIRouter.factory

```

1) #create wsgi service

```
server = service.WSGIService('osapi_compute')   --->
```

2)# load WSGI applications from paste configurations

```
wsgi.Loader()
self.app = self.loader.load_app(name)     --->
```

3)  # urlmap_factory, 

```
    for path, app_name in local_conf.items():
        path = paste.urlmap.parse_path_expression(path)
        app = loader.get_app(app_name, global_conf=global_conf)   --->
        urlmap[path] = app

```

4) #pipeline_factory

``` 
    app = loader.get_app(pipeline[-1])   -->
    filters.reverse()
    for filter in filters:
        app = filter(app)
    return app
```

这里的重要之处在于，通过app = filter(app) 将后面的filter app作为前面的middleware的构造初始化，从而形成整个call stack，比如

```
keystone = faultwrap sizelimit authtoken keystonecontext ratelimit osapi_compute_app_v2
app = ratelimit(osapi_compute_app_v2)
```

这样ratelimit middleware的就会call osapi_compute_app_v2，
所以 3) 返回了第一个middleware的urlmap，从而 WSGI server可以在接收到/v2/***
进入middleware的 call stack ，最终根据APIRouter的url和controller的映射配置，进行API 的request的具体处理。

5) #APIRouter
  
```
  class APIRouter(nova.api.openstack.APIRouter):
           ExtensionManager = extensions.ExtensionManager


 nova.api.openstack.APIRouter  --->
         if ext_mgr is None:
            if self.ExtensionManager:
                ext_mgr = self.ExtensionManager()   --->
            else:
                raise Exception(_("Must specify an ExtensionManager class"))
        mapper = ProjectMapper()
        self.resources = {}
        self._setup_routes(mapper, ext_mgr)    (6)
        self._setup_ext_routes(mapper, ext_mgr)  (7)
        self._setup_extensions(ext_mgr)    (8)
 
   
:ExtensionManager
self._load_extensions()   --->
load_extension()  ---> 

:ExtensionDescriptor

 ext_mgr.register(self)


:ExtensionManager

def _load_extensions(self):
        """Load extensions specified on the command line."""

        extensions = list(self.cls_list)

        for ext_factory in extensions:
            try:
                self.load_extension(ext_factory)
            except Exception as exc:
                LOG.warn(_('Failed to load extension %(ext_factory)s: '
                           '%(exc)s') % locals())


After all extensions have been loaded, (those extensions are configured in nova.conf, for example,
osapi_compute_extension=nova.api.openstack.compute.contrib.standard_extensions
```

6)   self._setup_routes(mapper, ext_mgr)

```
  for openstack core api, setup necessary routes
```

7)   self._setup_ext_routes(mapper, ext_mgr) 

```
 for each resource in  ext_mgr.get_resources()  
    
  wsgi_resource = wsgi.Resource(resource.controller,
                                          inherits=inherits)  
  
 :Resource
  self.register_actions(controller)

 # Generate routes for a controller resource
 #  mapping a resource is about handling creating, viewing, and editing that resource

  mapper.resource(resource.collection, resource.collection, **kargs)

```

8)    self._setup_extensions(ext_mgr)

```
for each extension in returned ControllerExtension objects
  get_controller_extensions
  ...
  resource = self.resources[collection]
  resource.register_actions(controller)
  resource.register_extensions(controller)



:ExtensionDescriptor  (append here for easy reference)

   def get_resources(self):
        """List of extensions.ResourceExtension extension objects.

        Resources define new nouns, and are accessible through URLs.

        """
        resources = []
        return resources

    def get_controller_extensions(self):
        """List of extensions.ControllerExtension extension objects.

        Controller extensions are used to extend existing controllers.
        """
        controller_exts = []
        return controller_exts
```

9)   manage a WSGI server, serving a WSGI application    --->

```
        self.server = wsgi.Server(name,
                                  self.app,
                                  host=self.host,
                                  port=self.port)
        service.serve(server, workers=server.workers)
```