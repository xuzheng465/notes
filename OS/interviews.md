1. 互斥锁（mutex）机制，以及互斥锁和读写锁的区别

互斥锁：mutex，用于保证在任何时刻，都只能有一个线程访问该对象。当获取锁操作失败时，线程会进入睡眠，等待锁释放时被唤醒

读写锁：分读锁和写锁。允许多个线程获得读锁。只能有一个线程可以获得写锁。。

写者优先于读者（一旦有写者，则后续读者必须等待，唤醒时优先考虑写者）



2. Linux的四种锁机制

   互斥锁

   读写锁

   自旋锁：spinlock，在任何时刻同样只能有一个线程访问对象。当时当获取锁操作失败时，不会进入睡眠，而是会在原地自旋，直到锁被释放。这样节省了线程从睡眠状态到被唤醒期间的消耗，在加锁时间短暂的环境下会极大提高效率。但如果加锁时间过长，则会非常浪费CPU资源

   RCU： read-copy-update. 在修改数据时，首先需要读取数据，然后生成一个副本，对副本进行修改。修改完成后，再将老数据update成新的数据。使用RCU时，读者几乎不需要同步开销，也不使用原子指令，不会导致锁竞争，因此就不用考虑死锁的问题了。写者的同步开销较大，它需要复制被修改的数据，还必须使用锁机制同步并行其他写者的修改操作。



## 进程与线程的区别

* 一个线程只能属于一个进程，一个进程可以有多个线程，但至少有一个线程。线程依赖于进程而存在
* 进程在执行过程中拥有独立的内存单元，而多个线程共享进程的内存。（资源分配给进程，同一进程的所有线程共享该线程的所有资源。同一进程中的多个线程共享代码段（代码和常量），数据段（全局变量和静态变量），扩展段（堆存储）。但是每个线程都拥有自己的栈段，栈段又叫运行时段，用来存放所有局部变量和临时变量。
* 进程是资源分配的最小单位，线程是CPU调度的最小单位。
* 系统开销 。由于在创建或撤销进程时，系统都要为之分配或回收资源，如内存空间，IO设备等。因此操作系统所付出的开销显著地大于在创建或撤销线程时的开销
* 在进行线程切换时，涉及到整个当前进程CPU环境的保存以及新被调度运行的进程的CPU环境的设置。而线程切换只需保存和设置少量寄存器的内容，并不涉及存储器管理方面的操作。可见，进程切换的开销也远大于线程切换的开销。



## 用户态和内核台区别

用户态和内核态是操作系统的两种运行级别，两者最大的区别是特权级不同。运行在用户态的程序不能直接访问操作系统内核数据结构和程序。

内核态和用户态之间的转换方式主要包括**系统调用**、**异常**和**中断**。



## 常用线程模型

### Future模型

Future是把结果放在将来获取，当前主线程并不急于获取处理结果。允许子线程先进行处理一段时间，处理结束之后就把结果保存下来，当主线程需要使用的时候再向子线程索取



### fork & join模型



