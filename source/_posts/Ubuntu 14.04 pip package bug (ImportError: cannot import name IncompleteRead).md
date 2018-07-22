title: Ubuntu 14.04 pip package bug (ImportError: cannot import name IncompleteRead)
date: 2015-05-18 11:35:49
tags: Python/Ruby
---


						ubuntu 14.04 默认安装的pip package有个bug，
运行pip 出现
Traceback (most recent call last):
  File "/usr/bin/pip", line 9, in 
    load_entry_point('pip==1.5.4', 'console_scripts', 'pip')()
  File "/usr/lib/python2.7/dist-packages/pkg_resources.py", line 351, in load_entry_point
    return get_distribution(dist).load_entry_point(group, name)
  File "/usr/lib/python2.7/dist-packages/pkg_resources.py", line 2363, in load_entry_point
    return ep.load()
  File "/usr/lib/python2.7/dist-packages/pkg_resources.py", line 2088, in load
    entry = __import__(self.module_name, globals(),globals(), ['__name__'])
  File "/usr/lib/python2.7/dist-packages/pip/__init__.py", line 61, in 
    from pip.vcs import git, mercurial, subversion, bazaar  # noqa
  File "/usr/lib/python2.7/dist-packages/pip/vcs/mercurial.py", line 9, in 
    from pip.download import path_to_url
  File "/usr/lib/python2.7/dist-packages/pip/download.py", line 25, in 
    from requests.compat import IncompleteRead
ImportError: cannot import name IncompleteRead

http://flexget.com/wiki/PipProblems
http://stackoverflow.com/questions/27341064/how-do-i-fix-importerror-cannot-import-name-incompleteread

都提到了这个问题，简单fix的方法，就是升级pip版本                                   