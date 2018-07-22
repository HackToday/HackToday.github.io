title: 谈谈Nova(osapi_compute)-extension
date: 2013-07-04 22:57:55
tags: 云计算
---


	
		*****************************************************************
	
	
		[composite:osapi_compute]
	
	
		use = call:nova.api.openstack.urlmap:
/: oscomputeversions
/v1.1: openstack_compute_api_v2
/v2: openstack_compute_api_v2
	
	
		
	
	
		
	
	
		
	
	
		[composite:openstack_compute_api_v2]
use = call:nova.api.auth:
noauth = faultwrap sizelimit noauth ratelimit osapi_compute_app_v2
keystone = faultwrap sizelimit authtoken keystonecontext ratelimit osapi_compute_app_v2
keystone_nolimit = faultwrap sizelimit authtoken keystonecontext osapi_compute_app_v2
	
	
		
	
	
		
	
	
		[app:osapi_compute_app_v2]
paste.app_factory = nova.api.openstack.compute:.factory
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		server = service.WSGIService('osapi_compute')   --->
	
	
		
	
	
		
	
	
		2)# load WSGI applications from paste configurations
	
	
		wsgi.Loader()
	
	
		self.app = self.loader.load_app(name)     --->
	
	
		
	
	
		
	
	
		3)  
	
	
		    for path, app_name in local_conf.items():
        path = paste.urlmap.parse_path_expression(path)
        app = loader.get_app(app_name, global_conf=global_conf)   --->
        urlmap[path] = app
	
	
		
	
	
		
	
	
		4) #
	
	
		 
	
	
		    app = loader.get_app(pipeline[-1])   -->
    filters.reverse()
    for filter in filters:
        app = filter(app)
	
	
		    return app
	
	
		
	
	
		这里的重要之处在于，通过app = filter(app) 将后面的filter app作为前面的middleware的构造初始化，从而形成整个call stack， 比如
	
	
		
	
	
		
	
	
		osapi_compute_app_v2)
	
	
		这样ratelimit middleware的就会call osapi_compute_app_v2，
	
	
		所以 3) 返回了第一个middleware的urlmap，从而 WSGI server可以在接收到/v2/***
	
	
		进入middleware的 call stack ，最终根据APIRouter的url和controller的映射配置，进行API 的request的具体处理。
	
	
		
	
	
		5) #
	
	
		 
	
	
		  class APIRouter(nova.api.openstack.APIRouter):
	
	
		           ExtensionManager = extensions.ExtensionManager
	
	
		
	
	
		
	
	
		 nova.api.openstack.APIRouter  --->
	
	
		         if ext_mgr is None:
	


	
		        mapper = ProjectMapper()
	


	
		 
	
	
		   
	
	
		:ExtensionManager
	
	
		self._load_extensions()   --->
	
	
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			def _load_extensions(self):
        """Load extensions specified on the command line."""

        

        for ext_factory in extensions:
            try:
                
            except Exception as exc:
                LOG.warn(_('Failed to load extension %(ext_factory)s: '
                           '%(exc)s') % locals())
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			self._setup_routes(mapper, ext_mgr)
		
		
			  for openstack core api, setup necessary routes
		
		
			
		
		
			7)   self._setup_ext_routes(mapper, ext_mgr) 
		
		
			
		
		
			 for each resource in  ext_mgr.get_resources()  
		
		
			    
		
		
			  wsgi_resource = wsgi.Resource(resource.controller,
		
                                          inherits=inherits)  
		
			  
		
		
			 :Resource
		
		
			  self.register_actions(controller)
		
		
			
		
		
			 mapping a resource is about handling creating, viewing, and editing that resource
		
		
			
		
		
			
		
		
			
		
		
			 self._setup_extensions(ext_mgr)
		
		
			for each extension in returned ControllerExtension objects
		
		
			
		
		
			  ...
		
		
			  resource = self.resources[collection]
		
  resource.register_actions(controller)
  resource.register_extensions(controller)
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			 manage a WSGI server, serving a WSGI application    --->
		
		
			
		
	
	
		
	
	
		
	


		*****************************************************************
	
		[composite:osapi_compute]
	
		use = call:nova.api.openstack.urlmap:
/: oscomputeversions
/v1.1: openstack_compute_api_v2
/v2: openstack_compute_api_v2
	
		
	
		
	
		
	
		[composite:openstack_compute_api_v2]
use = call:nova.api.auth:
noauth = faultwrap sizelimit noauth ratelimit osapi_compute_app_v2
keystone = faultwrap sizelimit authtoken keystonecontext ratelimit osapi_compute_app_v2
keystone_nolimit = faultwrap sizelimit authtoken keystonecontext osapi_compute_app_v2
	
		
	
		
	
		[app:osapi_compute_app_v2]
paste.app_factory = nova.api.openstack.compute:.factory
	
		
	
		
	
		
	
		
	
		
	
		server = service.WSGIService('osapi_compute')   --->
	
		
	
		
	
		2)# load WSGI applications from paste configurations
	
		wsgi.Loader()
	
		self.app = self.loader.load_app(name)     --->
	
		
	
		
	
		3)  
	
		    for path, app_name in local_conf.items():
        path = paste.urlmap.parse_path_expression(path)
        app = loader.get_app(app_name, global_conf=global_conf)   --->
        urlmap[path] = app
	
		
	
		
	
		4) #
	
		 
	
		    app = loader.get_app(pipeline[-1])   -->
    filters.reverse()
    for filter in filters:
        app = filter(app)
	
		    return app
	
		
	
		这里的重要之处在于，通过app = filter(app) 将后面的filter app作为前面的middleware的构造初始化，从而形成整个call stack， 比如
	
		
	
		
	
		osapi_compute_app_v2)
	
		这样ratelimit middleware的就会call osapi_compute_app_v2，
	
		所以 3) 返回了第一个middleware的urlmap，从而 WSGI server可以在接收到/v2/***
	
		进入middleware的 call stack ，最终根据APIRouter的url和controller的映射配置，进行API 的request的具体处理。
	
		
	
		5) #
	
		 
	
		  class APIRouter(nova.api.openstack.APIRouter):
	
		           ExtensionManager = extensions.ExtensionManager
	
		
	
		
	
		 nova.api.openstack.APIRouter  --->
	
		         if ext_mgr is None:
	
		        mapper = ProjectMapper()
	
		 
	
		   
	
		:ExtensionManager
	
		self._load_extensions()   --->
	
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			def _load_extensions(self):
        """Load extensions specified on the command line."""

        

        for ext_factory in extensions:
            try:
                
            except Exception as exc:
                LOG.warn(_('Failed to load extension %(ext_factory)s: '
                           '%(exc)s') % locals())
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			self._setup_routes(mapper, ext_mgr)
		
		
			  for openstack core api, setup necessary routes
		
		
			
		
		
			7)   self._setup_ext_routes(mapper, ext_mgr) 
		
		
			
		
		
			 for each resource in  ext_mgr.get_resources()  
		
		
			    
		
		
			  wsgi_resource = wsgi.Resource(resource.controller,
		
                                          inherits=inherits)  
		
			  
		
		
			 :Resource
		
		
			  self.register_actions(controller)
		
		
			
		
		
			 mapping a resource is about handling creating, viewing, and editing that resource
		
		
			
		
		
			
		
		
			
		
		
			 self._setup_extensions(ext_mgr)
		
		
			for each extension in returned ControllerExtension objects
		
		
			
		
		
			  ...
		
		
			  resource = self.resources[collection]
		
  resource.register_actions(controller)
  resource.register_extensions(controller)
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			
		
		
			 manage a WSGI server, serving a WSGI application    --->
		
		
			
		
	
			
		
			
		
			
		
			
		
			
		
			
		
			
		
			
		
			
		
			def _load_extensions(self):
        """Load extensions specified on the command line."""

        

        for ext_factory in extensions:
            try:
                
            except Exception as exc:
                LOG.warn(_('Failed to load extension %(ext_factory)s: '
                           '%(exc)s') % locals())
		
			
		
			
		
			
		
			
		
			
		
			
		
			self._setup_routes(mapper, ext_mgr)
		
			  for openstack core api, setup necessary routes
		
			
		
			7)   self._setup_ext_routes(mapper, ext_mgr) 
		
			
		
			 for each resource in  ext_mgr.get_resources()  
		
			    
		
			  wsgi_resource = wsgi.Resource(resource.controller,
		
			  
		
			 :Resource
		
			  self.register_actions(controller)
		
			
		
			 mapping a resource is about handling creating, viewing, and editing that resource
		
			
		
			
		
			
		
			 self._setup_extensions(ext_mgr)
		
			for each extension in returned ControllerExtension objects
		
			
		
			  ...
		
			  resource = self.resources[collection]
		
			
		
			
		
			
		
			
		
			
		
			
		
			
		
			
		
			 manage a WSGI server, serving a WSGI application    --->
		
			
		
		
	
		
	