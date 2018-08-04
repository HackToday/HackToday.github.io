title: 'openstack winpdb调试'
date: 2012-09-23 10:53:21
tags: 云计算
---


http://lists.openstack.org/pipermail/openstack-dev/2012-August/000794.html
看到了关于winpdb的相关调试：

Launch Winpdb GUI # winpdb Start Debugging

```
# cd /home/openstack-dev/workspace/nova/bin 
# winpdb -d -r 
```

Set Password: openstack

Attach to Process

1) In Winpdb GUI, select File -> Attach 
2) Enter password from step 4, should display files associated with that password 
3) Select process/file then select OK


To Stop Debug or Make Changes and Restart 

1) File -> Stop to detach current debugger 
2) Make changes in Eclipse and Save (make sure development user has write privileges to repository) 
3) kill and restart winpdb debugging process started above 4) Re-attach to the process in the Winpdb GUI 

也许自己不太会用，感觉比较难用。而且有时就崩溃了。还是eclipse方便些。
