静态内存：

* 用来保存局部static对象
* 类static数据成员
* 定义在任何函数之外的变量

栈内存：

* 保存定义在函数内的非static对象



分配在**静态**或**栈内存**中的对象由编译器自动创建和销毁。

对于**栈**对象，仅在其定义的程序块运行时才存在

**static**对象在使用之前分配，在程序结束时销毁



除了这两个，每个程序还拥有一个内存池。

自由空间，`free store` 或者叫做 堆 `heap`



如果你从`new`得到一个指针，你必须一直跟踪它。在某个节点必须调用delete 方法

*当有别人复制它会发生什么*

*如果本地变量过早跑出范围会发生什么？*

手动内存管理很困难，经常会犯如下错误

- Delete too soon
- Delete twice
- Never delete



### Rule of three

* Destructor
  * Delete what may have been created with `new`
* Copy constructor
  * Uses new to initialize from existing value
* Copy assignment operator
  * Deletes, th;en uses new to initialize



### Become Rule of Five

Move constructor

Move assignment operator



### Easy Memory Management



## Standard Library Smart Pointers

* unique_ptr
  * Noncopyable (use `std::move`)
* shared_ptr
  * Reference counted
* weak_ptr
  * lets you peek at a shared_ptr without bumping the reference count



> The free store (aka the heap) gives objects a lifetime longer than local scope

