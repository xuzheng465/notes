### Java 虚拟机内存划分

<img src="/Users/xuzheng/Projects/notes/Java/JVM/tsing.assets/内存区域.png" alt="内存区域" style="width:700px;" />



#### 程序计数器

* JVM将这个计数看作当前线程执行某条字节码的行数，会根据计数器的值来选去需要执行的操作语句。这个属于线程私有，不可共享，如果共享会导致技术混乱，无法准确的执行当前线程需要执行的语句

* 该区域不会出现outofmemory的情况

#### 虚拟机栈

* 虚拟机栈就是指经常说到的栈内存。Java中的每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程
* 如果线程的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常；如果虚拟机栈可以动态扩展（当前大部分Java虚拟机都可动态扩展，只不过Java虚拟机规范中也允许固定长度的虚拟机栈），如果扩展时无法申请到足够的内存，就会抛出OutOfMemoryError异常。

#### 本地方法栈

* 本地方法栈用来执行本地方法，抛出异常的情况和虚拟机栈一样。
* 而虚拟机栈用来执行Java方法

#### 堆

* 是JVM中内存最大、线程共享的一块区域。唯一的目的是存储对象实例。这里也是垃圾收集器的主收集的区域。
* 由于现代垃圾收集器采用的是分代收集算法，所以Java堆也分为新生代和老年代。
* 通过参数-Xmx（JVM最大可用内存）和-Xms（JVM初始内存）来调整堆内存，如果扩大至无法继续扩展时，会出现OutOfMemoryError的错误

#### 方法区

* JVM中内存共享的一片区域，用来存储类信息、常量、静态变量、class文件。
* 垃圾收集器也会对这部分区域进行回收，比如常量池的清理和类型的卸载
* 方法区内存不够用的时候， OutOfMemoryError



### 6.3 Java虚拟机类加载机制

#### 虚拟机类加载机制的概念

* 虚拟机把描述类的数据从class文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型

* Java语言里，类型的**加载和连接**过程是在程序运行期间完成

  

#### 类的生命周期

##### 加载 Loading

* 通过一个类的全限定名来获取此类的二进制字节码

* 将这个字节码所代表的静态存储结构转化为**方法区**的运行时数据结构。

* 在Java堆中生成一个代表这个类的Class对象，作为方法区这些数据的访问入口

  

##### 验证

* 虚拟机规范：验证输入的字节流是否符合Class文件的存储格式，否则抛出一个java.lang.VerifyError异常
* 文件格式验证：验证字节流是否符合Class文件格式的规范，并且能被当前版本的虚拟机处理。经过这个阶段的验证，字节流进入内存的方法区中进行存储
* 元数据验证：对类的元数据信息进行语义校验，保证不存在不符合Java语言规范的元数据信息
* 字节码验证：进行数据流和控制流的分析，对类的方法体进行校验分析，保证被校验的类的方法在运行时不会做出危害虚拟机安全的行为
* 符号引用验证：发生在虚拟机符号引用转化为直接引用的时候（解析阶段），对常量池中的各种符号引用的信息进行匹配行的校验

##### 准备

* 准备阶段是正式为**类变量**分配内存并设**置类变量**初始值（各数据类型的零值）的阶段，这些内存将在方法区中进行分配。但是如果类字段的字段属性表中存在ConstantValue属性，那在准备阶段变量值就会初始化为ConstantValue属性指定的值。

##### 解析

* 解析阶段是在虚拟机将常量池内的**==符号引用==**替换为==**直接引用**==的过程
* **符号引用**：符号引用以一组符号来描述所引用的目标，符号可以是任何形式的**字面量**，只要使用时能无歧义地定位到目标即可。符号引用与虚拟机实现的内存布局无关，引用的目标并不一定已经加载到内存中

##### 初始化

* `<clinit>()` 方法：由编译器自动收集类中所有类变量的赋值动作和静态语块中语句合并产生，收集的顺序是由语句在源文件中出现的顺序决定的。

* 该方法与实例构造器`<init>()` 不同，不需要显示的调用父类构造器
* `<clinit>`方法对于类或接口来说不是必须的

* 执行接口的`<clinit>()`不需要先执行父接口的`<clinit>()` 方法
* 虚拟机会保证一个类的`<clinit>()`方法在多线程环境中被正确的加载与同步



##### 类的主动引用

* 遇到new, getstatic, putstatic, invokestatic这四条字节码指令时（使用new实例化对象的时候，读取，设置一个类的静态字段、调用一个类的静态方法）
* 使用`java.lang.reflect`包的方法对类进行反射调用的时候
* 当初始化一个类的时候，如果发现其父类没有进行过初始化，则需要先触发其父类的初始化。
* 当虚拟机启动时，虚拟机会初始化主类（包含main方法的那个类）

##### 类的被动引用

* 通过子类引用父类的静态字段，不会导致子类初始化（对于静态字段，只有直接定义这个字段的类才会被初始化）
* 通过数组定义类应用类：ClassA[] arr = new ClassA[10]. 触发了一个名为【LClassA的类的初始化，它是一个由虚拟机自动生成的，直接继承于Object的类，创建动作由字节码指令newarray出发

### 6.4 判断对象是否存活算法及对象引用

#### 什么是垃圾回收

* 当一个对象没有引用指向它时，这个对象就称为无用的内存（垃圾），就必须进行回收，以便用于后续其他对象的内存分配。

#### 引用计数算法

* 实现简单，判断效率很高，在大部分情况下它都是一个不错的算法。但是Java语言中没有选用引用计数算法来管理内存，其中最主要的一个原因是它很难解决==**对象之间相互循环引用**==的问题。

#### 可达性分析算法（根搜索算法）

* 在主流的商用程序语言中（Java和C#），都是可达性分析判断对象是否存活的。
* 根搜索算法是从离散数学中的图论引入的，程序把所有的引用关系看作是一张图，从一个节点GC Root开始，如果一个节点与GC Root之间没有引用链存在，则该节点视为垃圾回收的对象。

* 在Java中， 可以作为GC root对象包括如下几种

  * 虚拟机栈（栈帧中的本地变量表）中引用的对象
  * 方法区中的类静态属性引用的对象
  * 方法区中的常量引用的对象
  * 本地方法栈中==JNI==的引用对象

  JNI   Java Native Interface

#### 对象引用-强引用

* 只要引用存在，垃圾回收器永远不会回收
  * Object obj = new Object();
* obj对象对后面new Object有一个强引用，只有当obj这个引用被释放后，对象才会被释放掉

#### 对象引用-软引用

* 非必须引用，内存溢出之前进行回收，
* 软引用主要用户实现类似缓存的功能，在内存足够的情况下直接通过软引用取值，无需从繁忙的真实来源查询数据，提升速度；**当内存不足时**，自动删除这部分缓存数据，从真正的来源查询这些数据。

对象引用-若引用

* 在第二次垃圾回收时回收，可以通过如下代码实现

  * ```java
    Object obj = new Object();
    WeakReference<Object> wf = new WeakReference<Object>(obj);
    obj = null;
    wf.get(); // 有时候会返回null
    wf.isEnQueued();
    ```

* 弱引用主要用于监控对象是否已经被垃圾回收器标记为即将回收的垃圾，可以通过弱引用的`isEnQueued`方法返回对象是否被垃圾回收回

### 6.5 分代垃圾回收

#### 年轻代和老年代

* 年轻代： 新创建的对象都存放在这里。因为大多数对象很快变得不可达，所以大多数对象在年轻代中创建，然后消失。当对象从这块内存区域消失时，我们说发生了一次‘minor GC’
* 老年代： 没有变得不可达，存活下来的年轻代对象被复制到这里。这块内存区域一般大于年轻代。因为它更大的规模，GC发生的次数比在年轻代的少。对象从老年代消失时，我们说“major GC” 或full GC发生了



#### 年轻代组成部分

* 年轻代3块区域，1块为Eden区，2块为Survivor区。各个空间的执行顺序如下：
  * 绝大多数新创建的对象分配在Eden区
  * 在Eden区发生一次GC后，存活的对象移动到其中一个Survivor区
  * 一旦一个Survivor区已满，存活的对象移动到另一个Survivor区。然后之前那个空间已满Survivor区将置为空，没有任何数据。
  * 经过重复多次这样的步骤后依旧存活的对象将被移到老年代。
* 





### 6.6 典型的垃圾收集算法

#### Mark-Sweep 标记-清除算法

<img src="/Users/xuzheng/Projects/notes/Java/JVM/tsing.assets/image-20210113112531688.png" alt="image-20210113112531688" style="width:700px;" />

* 最简单，最易实现
  * 标记阶段和清除阶段
  * 标记阶段的任务是标记出所有需要被回收的对象
  * 清除阶段就是回收被标记的对象所占用的空间
* 容易产生内存碎片。
* 碎片太多可能会导致后续过程中需要为大对象分配空间时，无法找到足够的空间而提前触发一次垃圾收集动作。

#### 复制算法 Copying

* 他将可用内存按照容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把自己的内存空间一次清理掉，这样一来就不容易出现内存碎片的问题。
* 这种算法虽然实现简单，运行高效且不容易产生内存碎片，但是缺对内存空间的使用做出了高昂的代价，因为能够使用的内存缩减到原来的**一半**。
* 如果存活数量很多，此算法的效率也会大大降低。

#### Mark-Compact 标记整理算法



#### Generational Collection （分代收集） 算法

* ==分代收集==算法是目前大部分JVM的垃圾收集器采用的算法。
  * 根据对象存活的生命周期将内存划分为若干个不同的区域。
  * 一般情况下将堆区划分为==老年代（Tenured Generation）==和==新生代（Young Generation）==
  * **老年代**的特点是每次垃圾收集时只有**少量**对象需要被回收，
  * **新生代**的特点是每次垃圾回收时都有**大量**的对象要被回收，那么就可以根据不同代的特点采取最适合的收集算法

* 新生代采取Copying算法。
  * 因为新生代中美次垃圾回收都要回收大部分对象，也就是说需要复制的操作次数较少，但是实际中不是按照1:1的比例来划分新生代空间的
  * 新生代氛围一块较大的Eden空间和两块较小的Survivor空间，每次使用Eden空间和其中一块Survivor空间，当进行回收时，将Eden和Survivor中还存活的对象复制到==**另一块==S**urvivor空间中，然后清理掉Eden和刚才使用过的Survivor空间。
* 老年代的特点是美次回收少量对象，一般使用的是mark-compact算法。
* 



### 6.7 典型的垃圾收集器



## 第七章 深入集合Collection

### 导学







### 集合框架与ArrayList





### LinkedList







### HashMap与HashTable





### TreeMap 与 LinkedHashMap







### HashSet







## 第八章 反射与代理机制



### 导学





### Java反射机制





### Java静态代理





### Java动态代理








