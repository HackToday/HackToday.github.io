title: Go语言编程的基本学习
date: 2015-10-07 18:31:59
tags: C/C++
---


	指针： Go 没有指针运算，不同于C

	

	数组：  Go's arrays are values. An array variable denotes the entire array; it is not a pointer to the first array element (as would be the case in C). This means that when you assign or pass around an array value you will make a copy of its contents. (To avoid the copy you could pass a  to the array, but then that's a pointer to an array, not an array.) One way to think about arrays is as a sort of struct but with indexed rather than named fields: a fixed-size composite value.

	

	Slice: ［1］指向一个序列的值，并且包含了长度信息。 使用make函数创建

	A slice is a descriptor of an array segment. It consists of a pointer to the array, the length of the segment, and its capacity (the maximum length of the segment).

	

	

	As we slice , observe the changes in the slice data structure and their relation to the underlying array:

	
		s = s[2:4]
	

		s = s[2:4]
	
	

	

	

	Range： 对于Slice和Map的for 迭代循环可使用range，可以通过_来忽略序号或者值

	

	    pow := make([]int, 10)

	    for i := range pow {

	        pow[i] = 1 << uint(i)

	    }

	    for _, value := range pow {

	        fmt.Printf("%d\n", value)

	    }

	Map： （待加）

	

	闭包： Go中函数也是值，也有闭包，这个在python中比较普遍。Go例子如下

	func fibonacci() func() int {

	    a := 0

	    b := 1

	    return func() int {

	    a ,    b = b, a + b

	        return a

	    }

	}

	

	方法：Go中没有类，但是可以对结构体定义方法

	比如：

	type Vertex struct {

	    X, Y float64

	}

	

	func (v *Vertex) Abs() float64 {

	    return math.Sqrt(v.X*v.X + v.Y*v.Y)

	}

	也可以对其他类型定义方法（除了基本类型和其他包的类型）

	 

	方法中使用指针接受者，是基于［4］：

	There are two reasons to use a pointer receiver. First, to avoid copying the value on each method call (more efficient if the value type is a large struct). Second, so that the method can modify the value that its receiver points to.

	

	关于method receiver的使用pointer还是value的问题解答［3］：

	－－－－－－－－－－－－－－－－－－－－－－－－

	

	
	
	
	
	
	

	 －－－－－－－－－－－－－－－－－－－－－－－－－－－－－

	

	接口： Go中的接口是一组方法定义的集合

	

	

	
	

	

	f(x, y, z)

	The evaluation of , , , and  happens in the current goroutine and the execution of  happens in the new goroutine.

	

	

	
	
	
		ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
	

		ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
	
	

	
	

	ch := make(chan int, 100)

	

	

	

	参考：

	1.  

	2.  

	3.  

	4.  
