1. JVM基础知识：
2. width:700px;

源代码跨平台

<img src="/Users/xuzheng/Projects/notes/Java/JavaCamp/Week1/notes.assets/image-20210106200924691.png" alt="image-20210106200924691" style="width:700px;" />

二进制跨平台

<img src="/Users/xuzheng/Projects/notes/Java/JavaCamp/Week1/notes.assets/image-20210106200602588.png" alt="image-20210106200602588" style="width:700px;" />



Java, C++, RUst 的区别

C/C++ 容易内存泄漏，程序崩溃，安全问题

Java/Golang 内存生命周期都有JVM运行时统一管理。有内存问题是，用JVM来进行信息相关的分析诊断和调整。

Rust 写起来不友好，生产力低。



源代码编译成字节码文件，虚拟机类加载器使其变成类， 有类了就有对象，如何划分内存空间



local variable LOAD stack

stack STORE local varaible(本地变量表)



```
javap -c -verbose Hello.class

javap -c Hello.class
```

常量池

本地变量表

只需要操作对象指针，常量就可以，所有操作转换成数值操作。

看方法变量表

```bash
javac -g Hello.java
javap -c -verbose Hello.class
```



Start 变量在哪行起作用。

Length 范围 比如 9 可以是0-8 范围



字节码前面的数字是byte的位置



JVM操作栈，栈操作数据

| 助剂   | 二进制 |
| ------ | ------ |
| new    | bb     |
| dup    | 57     |
| invode |        |

dup:复制栈顶一个字长的数据，将复制后的数据压栈

a开头 引用类型 对象

i开头 int



#### QA

字节码需要掌握到什么程度，？

字节码不是给人看的，是让JVM（计算机）无脑的去执行。



2. Java字节码技术

[字节码对照表](https://blog.csdn.net/u013294097/article/details/103747429)





3. JVM类加载器

三类加载器：

1. 启动类加载器 （BootstrapClassLoader）
2. 扩展类加载器（ExtClassLoader）
3. 应用类加载器（AppClassLoader）



加载器特点：

1. parent委托
2. 负责依赖
3. 缓存加载



4. JVM内存结构

内存屏障 保证每个cpu





5. JVM启动参数

系统属性参数

运行模式参数

堆内存设置参数

GC设置参数

分析诊断参数

JavaAgent参数



`-D` 设置系统属性

`-X`



`-XX`

