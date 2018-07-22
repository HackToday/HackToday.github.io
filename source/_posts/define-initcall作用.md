title: '__define_initcall 作用'
date: 2010-12-29 00:17:39
tags: LINUX
---


很好的一篇文章，所以转过来了，感谢前人的探索。
http://bigfirebird.iteye.com/blog/824818

```

前言 
宏定义__define_initcall(level,fn)对于内核的初始化很重要，他指示 
编译器在编译的时候，将一系列初始化函数的起始地址值按照一定的顺序 
放在一个section中。在内核初始化阶段，do_initcalls() 将按顺序从该 
section中以函数指针的形式取出这些函数的起始地址，来依次完成相应 
的初始化。由于内核某些部分的初始化需要依赖于其他某些部分的初始化 
的完成，因此这个顺序排列常常很重要。 
下面将从__define_initcall(level,fn) 宏定义的代码分析入手，依次 
分析名称为initcall.init的section的结构，最后分析内核初始化函数 
do_initcalls()是如何利用宏定义__define_initcall(level,fn)及其相 
关的衍生的7个宏宏定义，来实现内核某些部分的顺序初始化的。 
1、分析 __define_initcall(level,fn) 宏定义 
1) 这个宏的定义位于inlclude\linux\init.h中： 
#define __define_initcall(level,fn) \ 
static initcall_t __initcall_##fn \ 
__attribute__((__section__(".initcall" level ".init"))) \ 
= fn 
其中 initcall_t 是个函数指针类型： 
typedef int (*initcall_t)(void); 
而属性 __attribute__((__section__())) 则表示把对象放在一个这个 
由括号中的名称所指代的section中。 
所以这个宏定义的的含义是：1) 声明一个名称为__initcall_##fn的函数 
指针(其中##表示替换连接，)；2) 将这个函数指针初始化为fn；3) 编译 
的时候需要把这个函数指针变量放置到名称为 ".initcall" level ".init" 
的section中(比如level="1"，代表这个section的名称是 ".initcall1.init")。 
2) 举例：__define_initcall(6, pci_init) 
上述宏调用的含义是：1) 声明一个函数指针__initcall_pic_init = pci_init； 
且 2) 这个指针变量__initcall_pic_init 需要放置到名称为 .initcall6.init 
的section中( 其实质就是将 这个函数pic_init的首地址放置到了这个 
section中)。 
3) 这个宏一般并不直接使用，而是被定义成下述其他更简单的7个衍生宏 
这些衍生宏宏的定义也位于 inlclude\linux\Init.h 中: 
#define core_initcall(fn) __define_initcall("1",fn) 
#define postcore_initcall(fn) __define_initcall("2",fn) 
#define arch_initcall(fn) __define_initcall("3",fn) 
#define subsys_initcall(fn) __define_initcall("4",fn) 
#define fs_initcall(fn) __define_initcall("5",fn) 
#define device_initcall(fn) __define_initcall("6",fn) 
#define late_initcall(fn) __define_initcall("7",fn) 
因此通过宏 core_initcall() 来声明的函数指针，将放置到名称为 
.initcall1.init的section中，而通过宏 postcore_initcall() 来 
声明的函数指针，将放置到名称为.initcall2.init的section中， 
依次类推。 
4) 举例：device_initcall(pci_init) 
解释同上 1－2)。 
2、和初始化调用有关section--initcall.init被分成了7个子section 
1) 他们依次是.initcall1.init、.initcall2.init、...、.initcall7.init 
2) 按照先后顺序依次排列 
3) 他们的定义在文档vmlinux.lds.S中 
例如 对于i386+，在i386\kernel\vmlinux.lds.S中有： 
__initcall_start = .; 
.initcall.init : { 
*(.initcall1.init) 
*(.initcall2.init) 
*(.initcall3.init) 
*(.initcall4.init) 
*(.initcall5.init) 
*(.initcall6.init) 
*(.initcall7.init) 
} 
__initcall_end = .; 
而在makefile 中有 

LDFLAGS_vmlinux += -T arch/$(ARCH)/kernel/vmlinux.lds.s 
4) 在这7个section总的开始位置被标识为__initcall_start， 
而在结尾被标识为__initcall_end。 
3、 内核初始化函数do_basic_setup(): do_initcalls() 将从.initcall.init 
中，也就是这7个section中依次取出任何的函数指针，并调用这些 
函数指针所指向的函数，来完成内核的一些相关的初始化。 
这个函数的定义位于init\main.c中： 
extern initcall_t __initcall_start, __initcall_end; 
static void __init do_initcalls(void) 
{ 
initcall_t *call; 
.... 
for (call = &__initcall_start; call 

*********************************************************************** 

假如您希望某个初始化函数在内核初始化阶段就被调用，那么您 
就应该使用宏__define_initcall(level,fn) 或 其7个衍生宏来 
把这个初始化函数fn的起始地址按照初始化的顺序放置到相关的 
section 中。 内核初始化时的do_initcalls()将从这个section 
中按顺序找到这些函数来执行。 假如您希望某个初始化函数在内核初始化阶段就被调用，那么您 
就应该使用宏__define_initcall(level,fn) 或 其7个衍生宏来 
把这个初始化函数fn的起始地址按照初始化的顺序放置到相关的 
section 中。 内核初始化时的do_initcalls()将从这个section 
中按顺序找到这些函数来执行。 

******************************************************************* 
http://forum.oss.org.cn/viewtopic.php?p=8899&sid=1f5aee1b543bfca24793e4508dd115d0 


今天在做一个驱动的时候要用到另一个驱动（I2C）提供的API，在内核初始化时碰到了一个依赖问题。 
　　我的驱动在I2C初始化之前就运行起来了，而这时I2C提供的API还处于不可用状态。查了很多资料，网上有人说任何使用module_init这个宏的驱动程式的起动顺序都是不确定的（我没有查到权威的资料）。 
　　任何的__init函数在区段.initcall.init中还保存了一份函数指针，在初始化时内核会通过这些函数指针调用这些__init函数指针，并在整个初始化完成后，释放整个init区段（包括.init.text，.initcall.init等）。 
　注意，这些函数在内核初始化过程中的调用顺序只和这里的函数指针的顺序有关，和1）中所述的这些函数本身在.init.text区段中的顺序无关。在 
2.4内核中，这些函数指针的顺序也是和链接的顺序有关的，是不确定的。在2.6内核中，initcall.init区段又分成7个子区段，分别是 
.initcall1.init 
.initcall2.init 
.initcall3.init 
.initcall4.init 
.initcall5.init 
.initcall6.init 
.initcall7.init 
　　当需要把函数fn放到.initcall1.init区段时，只要声明 
core_initcall(fn); 
　　即可。 
　　其他的各个区段的定义方法分别是： 
core_initcall(fn) --->.initcall1.init 
postcore_initcall(fn) --->.initcall2.init 
arch_initcall(fn) --->.initcall3.init 
subsys_initcall(fn) --->.initcall4.init 
fs_initcall(fn) --->.initcall5.init 
device_initcall(fn) --->.initcall6.init 
late_initcall(fn) --->.initcall7.init 
　而和2.4兼容的initcall(fn)则等价于device_initcall(fn)。各个子区段之间的顺序是确定的，即先调用. 
initcall1.init中的函数指针，再调用.initcall2.init中的函数指针，等等。而在每个子区段中的函数指针的顺序是和链接顺序相
关的，是不确定的。 
　　在内核中，不同的init函数被放在不同的子区段中，因此也就决定了他们的调用顺序。这样也就解决了一些init函数之间必须确保一定的调用顺序的问题。按照include/linux/init.h文档所写的，我在驱动里偿试了这样两种方式： 
__define_initcall("7", fn); 
late_initcall(fn); 
　　都能够把我的驱动调整到最后调用。实际上上面两个是一回事： 
#define late_initcall(fn) __define_initcall("7", fn)

```