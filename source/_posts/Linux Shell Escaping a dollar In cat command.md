title: 'Linux Shell Escaping a dollar In cat command'
date: 2015-07-24 08:02:49
tags: LINUX
---

发现社区一个脚本执行后，cat命令写入后文件里面的表达式都求值展看了，其实这个是因为$的缘故，参看这个

http://stackoverflow.com/questions/21984960/escaping-a-dollar-sign-in-unix-inside-the-cat-command

```
You can use regular quoting operators in a here document:

$ cat <<HERE
> foo(\$bar)
> HERE
foo($bar) 

or you can disable expansion by quoting the here-doc delimiter:

$ cat <<'HERE'  # note single quotes
> foo($bar)
> HERE
foo($bar)
```
