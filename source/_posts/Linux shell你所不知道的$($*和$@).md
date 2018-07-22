title: Linux shell你所不知道的$($*和$@)
date: 2013-07-30 21:06:21
tags: LINUX
---


	 
	
		
	
	
		
	
	
		
	
	
		1）* 和 @
	
	
		在shell中虽然都是展开位置参数可以用
	
	
		$@和$*，但是两种有很大的差别，特别是在双引号的扩展下，比如
	
	
		
	
	
		
	
	
		(a)
	
	
		for i  in $*; do
	
	
		     echo "--$i"
	
	
		done
	
	
		
	
	
		
	
	
		(b)
	
	
		for i in $@; do
	
	
		     echo "**$i"
	
	
		done
	
	
		
	
	
		如果传入的参数是"hi ni"
	
	
		
	
	
		对于(a),
	
	
		
	
	
		-- hi
-- ni

	
	
		
	
	
		对于(b)，
	
	
		
	
	
		** hi
** ni
	
	
		
	
	
		
	
	
		但是如果你说我传入的“hi ni”是一个参数（$1)，我不希望它输出为多个参数
	
	
		
	
	
		那么我们更改脚本，改为
	
	
		
	
	
		for i in "$*"; do
	
	
		    echo "--$1"
	
	
		done
	
	
		
	
	
		
	
	
		for i in "$@"; do
	
	
		    echo "**$i"
	
	
		done
	
	
		
	
	
		如果传入的参数是"hi ni"
	
	
		
	
	
		-- hi ni
	
 
	
		
	
	
		现在是相同的，没问题，但是如果传入的参数是空，那么你会发现下面的输出结果
	
	
		-- 
	
	
		
	
	
		
	
	
		这是因为（man bash)
	
	
		
	
	
		"$*" is equivalent to "$1c$2c...", where c is the first character of the value of the IFS variable.
	
	
		"$@" is equivalent to "$1" "$2" ... 
	
	
		
	
	
		显然是"$*"的展开是包含了一个空格，关于IFS的介绍，可以参看参考[1]
	
	
		
	
	
		同样，我们可以知道如果设置IFS的话，那么多个参数的情况下，"$*"和"$@"输出是不一致的，比如(如果设置IFS=",")
	
	
		输入参数， "hi ni" "you"
	
	
		-- hi ni,you
** hi ni
** you
	
	
		
	
	
		
	
	
		
	
	
		参考：
	
	
		[1]  
	
	
		
	

		
	
		
	
		
	
		1）* 和 @
	
		在shell中虽然都是展开位置参数可以用
	
		$@和$*，但是两种有很大的差别，特别是在双引号的扩展下，比如
	
		
	
		
	
		(a)
	
		for i  in $*; do
	
		     echo "--$i"
	
		done
	
		
	
		
	
		(b)
	
		for i in $@; do
	
		     echo "**$i"
	
		done
	
		
	
		如果传入的参数是"hi ni"
	
		
	
		对于(a),
	
		
	
		-- hi
-- ni

	
		
	
		对于(b)，
	
		
	
		** hi
** ni
	
		
	
		
	
		但是如果你说我传入的“hi ni”是一个参数（$1)，我不希望它输出为多个参数
	
		
	
		那么我们更改脚本，改为
	
		
	
		for i in "$*"; do
	
		    echo "--$1"
	
		done
	
		
	
		
	
		for i in "$@"; do
	
		    echo "**$i"
	
		done
	
		
	
		如果传入的参数是"hi ni"
	
		
	
		-- hi ni
	
		
	
		现在是相同的，没问题，但是如果传入的参数是空，那么你会发现下面的输出结果
	
		-- 
	
		
	
		
	
		这是因为（man bash)
	
		
	
		"$*" is equivalent to "$1c$2c...", where c is the first character of the value of the IFS variable.
	
		"$@" is equivalent to "$1" "$2" ... 
	
		
	
		显然是"$*"的展开是包含了一个空格，关于IFS的介绍，可以参看参考[1]
	
		
	
		同样，我们可以知道如果设置IFS的话，那么多个参数的情况下，"$*"和"$@"输出是不一致的，比如(如果设置IFS=",")
	
		输入参数， "hi ni" "you"
	
		-- hi ni,you
** hi ni
** you
	
		
	
		
	
		
	
		参考：
	
		[1]  
	
		
	