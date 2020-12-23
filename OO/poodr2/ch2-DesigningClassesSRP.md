Code should be

* **Transparent**

The consequences of change should be obvious in the code that is changing and in distant code that relies upon it.

* **Reasonable**

The cost of any change should be proportional to the benefits the change archieves

* **Usable**

Existing code should useable in new and unexpected contexts

* **Exemplary** 示范性

The code itself should encourage those who change it to perpetuate these qualities.

## 2.2 Creating classes That have a Single Responsibility

一个类应该做尽可能小的有用的事情，也就是说，它应该有单一的责任。

### 2.2.1 An Example Application: Bicycles and Gears



### 2.2.2 Why Single Responsibility Matters

易于变更的应用程序由易于重用的类组成。

可重用类是定义良好行为的可插拔单元，其很少有纠缠。

一个易于改变的应用程序就像一盒积木，你可以只选择你需要的部件，并以意想不到的方式组装它们。

有多于一个指责的类重用很困难。各种责任很可能在类内部纠缠不清。

### 2.2.3 Determining if a class Has a Single Responsibility

如何确定Gear这个类是否包含属于其他地方的方法（行为）。

**一种**方法是假装它是有意识的，然后审问它。如**果你把它的每一个方法都改写成一个问题，问问题应该是有意义的。**例如，"请问齿轮先生，你的传动比是多少？"似乎非常合理，而 "请问齿轮先生，你的齿轮_英寸是多少？"则是有些不太对，"请问齿轮先生，你的轮胎（尺寸）是多少？"简直是荒谬至极。

Don’t resist the idea that “What is your tire?” is a question that can legitimately be asked. From inside the Gear class, tire may feel like a different kind of thing than ratio or gear_inches, but that means nothing. From the point of view of every other object, anything that Gear can respond to is just another message. If Gear responds to it, someone will send it, and that sender may be in for a rude surprise when Gear changes.



**另一种**方法是尝试用一句话来描述

一个类应该做一个最小的有用的事。

这个事应该很简单就能描述出来。如果你能想出的最简洁的描述使用了“和”这个字，那么这个类很可能不止有一个责任。

如果用了“或”这个字，那么类的责任就不只有一个，他们甚至没有太大的关系



OO designer 使用**内聚**来描述这个概念。当类中的一切都与其主要目的有关，就可以说这个类具有高度的凝聚力，或者说这个类的责任是单一的。



SRP并不要求一个类只做一件非常狭隘的事情，也不要求它只为一个微不足道的原因而改变，相反，SRP要求一个类是有凝聚力的--这个类所做的一切都与它的目的高度相关。



### 2.2.4 Determining When to Make Design Decisions

如果你只知道未来会有哪些功能需求，你今天就可以做出完美的设计决策。任何事都可能发生。

在掷骰子之前，你可能会浪费很多时间在同样合理的选择之间徘徊，选择错误的一个。

不要觉得被迫过早地做出设计决定。

别这样，即使你担心你的代码会让设计大师们失望。

面对Gear这样不完美，muddled的类，请问你自己。**“今天什么都不做，未来的代价是什么？”**

