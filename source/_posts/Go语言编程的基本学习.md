title: 'Go语言编程的基本学习'
date: 2015-10-07 18:31:59
tags: C/C++
---

指针： Go 没有指针运算，不同于C

数组：  Go's arrays are values. An array variable denotes the entire array; it is not a pointer to the first array element (as would be the case in C). This means that when you assign or pass around an array value you will make a copy of its contents. (To avoid the copy you could pass a pointer to the array, but then that's a pointer to an array, not an array.) One way to think about arrays is as a sort of struct but with indexed rather than named fields: a fixed-size composite value.

Slice: ［1］指向一个序列的值，并且包含了长度信息。 使用make函数创建
A slice is a descriptor of an array segment. It consists of a pointer to the array, the length of the segment, and its capacity (the maximum length of the segment).


As we slice s, observe the changes in the slice data structure and their relation to the underlying array:
s = s[2:4]



Range： 对于Slice和Map的for 迭代循环可使用range，可以通过_来忽略序号或者值

```
    pow := make([]int, 10)
    for i := range pow {
        pow[i] = 1 << uint(i)
    }
    for _, value := range pow {
        fmt.Printf("%d\n", value)
    }
```

Map： （待加）

闭包： Go中函数也是值，也有闭包，这个在python中比较普遍。Go例子如下

```
func fibonacci() func() int {
    a := 0
    b := 1
    return func() int {
    a ,    b = b, a + b
        return a
    }
}
```

方法：Go中没有类，但是可以对结构体定义方法
比如：

```
type Vertex struct {
    X, Y float64
}

func (v *Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

也可以对其他类型定义方法（除了基本类型和其他包的类型）
 
方法中使用指针接受者，是基于［4］：

```
There are two reasons to use a pointer receiver. First, to avoid copying the value on each method call (more efficient if the value type is a large struct). Second, so that the method can modify the value that its receiver points to.
```

关于method receiver的使用pointer还是value的问题解答［3］：

```
Should I define methods on values or pointers?
func (s *MyStruct) pointerMethod() { } // method on pointer
func (s MyStruct)  valueMethod()   { } // method on value


For programmers unaccustomed to pointers, the distinction between these two examples can be confusing, but the situation is actually very simple. When defining a method on a type, the receiver (s in the above examples) behaves exactly as if it were an argument to the method. Whether to define the receiver as a value or as a pointer is the same question, then, as whether a function argument should be a value or a pointer. There are several considerations.

First, and most important, does the method need to modify the receiver? If it does, the receiver must be a pointer. (Slices and maps act as references, so their story is a little more subtle, but for instance to change the length of a slice in a method the receiver must still be a pointer.) In the examples above, if pointerMethod modifies the fields of s, the caller will see those changes, but valueMethod is called with a copy of the caller's argument (that's the definition of passing a value), so changes it makes will be invisible to the caller.

By the way, pointer receivers are identical to the situation in Java, although in Java the pointers are hidden under the covers; it's Go's value receivers that are unusual.

Second is the consideration of efficiency. If the receiver is large, a big struct for instance, it will be much cheaper to use a pointer receiver.

Next is consistency. If some of the methods of the type must have pointer receivers, the rest should too, so the method set is consistent regardless of how the type is used. See the section on method sets for details.

For types such as basic types, slices, and small structs, a value receiver is very cheap so unless the semantics of the method requires a pointer, a value receiver is efficient and clear.
```


接口： Go中的接口是一组方法定义的集合

```
Goroutines
A goroutine is a lightweight thread managed by the Go runtime.

go f(x, y, z)

starts a new goroutine running

f(x, y, z)
The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in the new goroutine.


Channels
Channels are a typed conduit through which you can send and receive values with the channel operator, <-.

ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.

Buffered Channels
Channels can be buffered. Provide the buffer length as the second argument to make to initialize a buffered channel:

ch := make(chan int, 100)
```

参考：

1. http://blog.golang.org/go-slices-usage-and-internals
2. http://nathanleclaire.com/blog/2014/08/09/dont-get-bitten-by-pointer-vs-non-pointer-method-receivers-in-golang/
3. https://golang.org/doc/faq
4. https://tour.golang.org/