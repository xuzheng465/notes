# 第二章 性能测试方法



## 测试真实应用



### 微基准测试

Designed to measure a small unit of performance in order to decide which of multiple alternate implementations is preferable:

1. 创建线程的代价与创建线程池的代价
2. 执行某种算法的耗时与其替代实现的耗时。

Microbenchmarks differ from regular programs in various ways. First, because Java code is interpreted the first few times it is executed, it gets faster the longer it is executed. For this reason, all benchmarks (not just microbenchmarks) typically include a warm-up period during which the JVM is allowed to compile the code into its optimal state.

Microbenchmarks与普通程序有不同的地方。首先，因为Java代码在执行的前几次都会被解释，所以执行的时间越长速度越快。出于这个原因，所有的基准（不仅仅是微基准）通常都包含一个预热期，在此期间，JVM可以将代码编译到最佳状态。

```java
public void doTest() { // Main Loop double l;

  for (int i = 0; i < nWarmups; i++) {

  	l = fibImpl1(50); 
  } long then = System.currentTimeMillis();

  for (int i = 0; i < nLoops; i++) {

  	l = fibImpl1(50); 
  } 
  long now = System.currentTimeMillis(); 
  System.out.println("Elapsed time: " + (now - then));

}
```

这段代码想要测量执行 fibImpl1()方法的时间，所以它先让编译器预热，然后测量现在编译的方法。但很有可能，这个时间会是0（或者更有可能是运行没有主体的for循环的时间）。由于l的值没有在任何地方被读取，编译器可以完全跳过它的计算。这取决于 fibImpl1()方法中还发生了什么，但如果只是一个简单的算术运算，就可以全部跳过。也有可能只执行该方法的部分内容，甚至可能产生l的错误值；由于该值从未被读取，所以没有人会知道

有一个方法可以解决这个特殊的问题：**确保每个结果都被读取**，而不是简单的写入。在实践中，将l的定义从局部变量改为实例变量（用**volatile**关键字声明），就可以衡量方法的性能。



