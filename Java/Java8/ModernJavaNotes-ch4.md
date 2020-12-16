# 第4章 流



What is Stream?

A sequence of elements from a source that supports data-processing operations.

从支持数据处理操作的源生成的元素序列。



集合是数据结构，它的主要目的是以特定的时间/空间复杂度存储和访问元素（ArrayList和LinkedLIst）。

流 stream目的在于表达计算。

集合讲的是数据。

流讲的是计算。



**源**： 流会使用一个提供数据的源，比如集合、数组或I/O资源。从有序集合生成流时会保留原有的顺序。

**数据处理操作**：流的数据处理功能支持类似于数据库的操作，以及函数式编程语言中的常用操作

比如`filter, map, reduce, find, match, sort`。

流操作可以顺序执行，也可以并行执行。

**流水线** -- 很多流操作本身会返回一个流，这样多个操作就可以链接起来，构成一个更大的流水线。可以处理**延迟**和**短路**。



**内部迭代** 与集合使用迭代器进行显式迭代不同，流的迭代操作是在后台进行的。



**集合与流的差异**：

* 在于什么时候进行计算。
* 集合是一个内存中的数据结构， 它包含数据结构中目前所有的值——集合中的每个元素都得先算出来才能添加到集合中
* 流则是在概念上固定的数据结构（你不能添加或删除元素），其元素是按需计算的。
  * 用户仅仅从流中提取需要的值，这些值--在用户看不见的地方--只会按需生成。
  * 流就像是一个延迟创建的集合：只有在消费者要求的时候才会计算值



**流只能遍历一次**

遍历之后，这个流就被消费掉了。

**外部迭代与内部迭代**

使用Collection接口需要用户去做迭代（比如用for-each），称为外部迭代。

Stream库使用内部迭代。

* stream帮你把迭代做了。
* 还把得到的流的值存在了某个地方
* 你说出个函数名说想要干啥就可以



* 外部迭代 for-each

  ```java
  List<String> names = new ArrayList<>();
  for(Dish dish: menu) {
    names.add(dish.getName());
  }
  ```

* 外部迭代 迭代器版本 （丑 啰嗦）

  ```java
  Iterator<String> iterator = menu.iterator();
  while(iterator.hasNext()) {
    Dish dish = iterator.next();
    names.add(dish.getName());
  }
  ```

* 流： 内部迭代

  ```java
  List<String> names = menu.stream()
    												.map(Dish::getName)
    												.collect(toList());
  ```

## 流操作

```java
List<String> names = menu.stream()	// 从菜单获得stream
  												.filter(dish -> dish.getCalories() > 300)
  												.map(Dish::getName)
  												.limit(3)
  												.collect(toList());
```

* filter，map和limit可以连成一条流水线；
* `collect`触发流水线执行并关闭它。

除非流水线上触发一个终端操作，否则中间操作不会执行任何处理——它们很 懒。



## 使用流

流的使用包括三件事：

1. 一个**数据源**（如集合）来执行一个查询；
2. 一个**中间操作链**，形成一条流的流水线
3. 一个**终端操作**，执行流水线，并能生成结果。



## 总结

* 流是 “从支持数据处理操作的源生成的一系列元素”

* 流利用内部迭代：迭代通过filter，map，sorted等操作被抽象掉了
* 流操作有两类：中间操作和终端操作。
* filter和map等中间操作会返回一个流，并可以链接在一起。可以用他们来设置一条流水线，并不会生成任何效果。

* forEach和count等终端操作会返回一个非流的值，并处理流水线以返回结果。
* 流中的元素是按需计算的

