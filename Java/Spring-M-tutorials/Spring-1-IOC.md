DAO

Data Access Object



IOC: 控制反转, 设计思想

AOP: 面向切面编程

容器: 包含并管理应用对象的生命周期, 就好比用桶装水一样. Spring就是桶, 对象是水

**基本概念**

```
	IoC is also known as dependency injection (DI). It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern.
	IOC与大家熟知的依赖注入同理，. 这是一个通过依赖注入对象的过程 也就是说，它们所使用的对象，是通过构造函数参数，工厂方法的参数或这是从工厂方法的构造函数或返回值的对象实例设置的属性，然后容器在创建bean时注入这些需要的依赖。 这个过程相对普通创建对象的过程是反向的（因此称之为IoC），bean本身通过直接构造类来控制依赖关系的实例化或位置，或提供诸如服务定位器模式之类的机制。
```



想要搞明白IOC，那么需要搞清楚如下几个问题：

```
1、谁控制谁
2、控制什么
3、什么是反转
4、哪些方面被反转
```



1. 谁控制谁: 之前是程序猿自己去new对象, 有了IOC容器后, 就会变成IOC容器来控制对象.

2. 控制什么: 在实现过程中所需要的对象及需要依赖的对象.

3. 什么是反转: 

   ```
   正转: 程序员在对象中主动去创建依赖的对象. 
   反转: 依赖的对象直接用IOC容器创建后注入到对象中, 由主动创建变成了被动接受
   ```

4. 哪些方面被反转: 依赖的对象



**DI与IOC**

```
IOC是设计思想, DI是具体的实现方式
IOC从容器角度描述, DI从应用程序的角度来描述
```



**容器中的对象是什么时候创建的?**

容器中的对象在容器创建完成之前就已经把对象创建好了



**搭建spring项目需要注意的点：**

​		1、一定要将配置文件添加到类路径中，使用idea创建项目的时候要放在resource目录下

​		2、导包的时候别忘了commons-logging-1.2.jar包

​		**细节点：**

​		1、ApplicationContext就是IOC容器的接口，可以通过此对象获取容器中创建的对象

​		2、对象在Spring容器创建完成的时候就已经创建完成，不是需要用的时候才创建

​		3、对象在IOC容器中存储的时候都是单例的，如果需要多例需要修改属性

​		4、创建对象给属性赋值的时候是通过**setter**方法实现的

​		5、**对象的属性是由setter/getter方法决定的**，而不是定义的成员属性

