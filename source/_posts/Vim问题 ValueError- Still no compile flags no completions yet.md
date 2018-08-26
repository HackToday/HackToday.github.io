title: 'Vim问题 ValueError: Still no compile flags, no completions yet'
date: 2016-10-19 21:53:36
tags: LINUX
---

这个是因为ycm_extra_conf没有提供给YouCompleteMe编译标志，具体参见这个issue

https://github.com/Valloric/YouCompleteMe/issues/700


解决方法：

Set below in vimrc works fine. (设置对应的.ycm_extra_conf.py的路径)
	
```
let g:ycm_global_ycm_extra_conf = '/root/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py' 
```
                                   