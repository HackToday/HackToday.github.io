title: DevOps-chef的hello-world cookbook
date: 2013-09-20 17:01:12
tags: 系统运维
---


	

	

4. 上传cookbook到chef server
   knife  cookbook  upload 'hello_world'

5. 将相应的cookbook添加到对应节点run_list
  knife node run_list add 
	

7. 节点应用cookbook
knife ssh name:
		
	
		
	