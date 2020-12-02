```java
public calss GCDRunnable implements Runnable {
  public void run() {
    // code to run goes here
  }
}
Runnable gCDruannable = new GCDRunnable();
new Thread(gCDrunnable).start();

// lambda expr
new Thread(()-> {
  // code to run
}).start();
// OR
Runnable r = () -> {
  // code
};
new Thread(r).start();
```

## 线程创建

### 实现runnable接口

```java
public class RunnableThread implements Runnable {
    @Override
    public void run() {
        System.out.println('用实现Runnable接口实现线程');
    }
}
```

把实现了这个`run()`方法的实例传到Thread类中就可以实现多线程.

### 继承Thread类

```java
public class ExtendsThread extends Thread {
    @Override
    public void run() {
        System.out.println('用Thread类实现线程');
    }
}
```

### 线程池创建

对于线程池而言，本质上是通过线程工厂创建线程的，默认采用 DefaultThreadFactory ，它会给线程池创建的线程设置一些默认值，比如：线程的名字、是否是守护线程，以及线程的优先级等。但是无论怎么设置这些属性，最终它还是通过 new Thread() 创建线程的 



### 线程池的好处

**相比new Thread，Java提供的四种线程池的好处在于：**

a. 重用存在的线程，减少对象创建、消亡的开销，性能佳。
b. 可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。
c. 提供定时执行、定期执行、单线程、并发数控制等功能。





### Join

THe `join()` method allows one thread to wait for another thread to complete.

A simple form of **barrier synchronization**



`void setDaemon()`: Marks thread as a "daemon"

If there is only a daemon thread, the program will shutdown

* `void start()`
  * Allocates thread resources & initiates thread execution by calling the run() hook method
* `void run()`
  * Hood method where user code is supplied
* 



#### Pro with extending Thread

* It's straightforward to exted the Thread super class
  * Just override the run() hook method
* All state and methods are consolidated in one place.
  * enables central allocation and management of the thread
  * The design is useful when the thread must be updated during runtime configuration changes
    * e.g., interrupting/restarting a running thread and readding / writing its state

#### Cons with extending Thread

* A subclass must extend the Thread superclass
  * This is restrictive since java only allows one superclass per subclass



#### Pros of implementing Runnable







Resource

https://juejin.cn/post/6844904117547057159