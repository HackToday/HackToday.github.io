title: chef的文件传输
date: 2014-03-23 17:40:20
tags: 系统运维
---


	
gem_package "test" do
  gem_binary("/opt/chef/embedded/bin/gem")
  source "/tmp/test.gem"
  action :install
end
	
		
	


		
	