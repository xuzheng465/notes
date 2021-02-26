**栈** 是存储器用来保存局部变量的区域。

数据保存在局部变量中，一旦离开函数，变量就会消失

<img src="/Users/xuzheng/Projects/notes/C/memoryManagement.assets/存储器-dm.png" alt="存储器-dm" style="width:500px;" />



**堆**是程序中用来保存长期使用数据的地方。

堆上的数据不会自动清除。

因此**堆**是保存数据结构的绝佳场所。



## malloc()

`malloc()`

memory allocation （存储器分配）

`malloc()` 接收一个参数：所需要的字节数





`strdup()` 做什么？

strdup() 函数可以把字符串复制到堆上。



