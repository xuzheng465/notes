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

==When the future cost of doing nothing is the same as the current cost, postpone the decision. Make the decision only when you must with the information you have at that time.==

当什么都不做的未来成本与当前所花成本相同时，推迟决策。

只有当你必须用当时所掌握的信息做出决定时，才去做出决定。

Even though there’s a good argument for leaving Gear as is <u>for the time being</u>, you could also make a defensible argument that it should be changed.

即使有充分的理由让Gear<u>暂时</u>保持原样， 你也可以理直气壮地提出应该改变它

The structure of every class is a message to future maintainers of the application.

每一个类的结构都是给未来程序维护者的信息。

It reveals your design intentions.

它揭示了你的设计意图。

For better or for worse, the patterns you establish today will be replicated forever.

无论好坏，你今天建立的模式将永远复制



Gear并没有阐释你的真实意图。它既没可用性也没示范性。它有多个职责，所以不应该被重用。这不是一种应该被复制的模式。



There is a chance that someone else will reuse Gear or create new code that follows its pattern while you are waiting for better information. 

在你等待更好的信息时，有可能别人会重用Gear或创造出遵循其模式的新代码

Other developers believe that your intentions are reflected in the code; 

其他开放者相信的你的意图会反应在代码中

when the code lies, you must be alert to programmers believing and then propagating that lie.

当代码说谎时，你必须警惕程序员相信并传播这个谎言。

This “improve it now” versus “improve it later” tension always exists. 

这种现在改进与以后改进的紧张关系始终存在

Applications are never perfectly designed. 

应用从来没有完美的设计。

Every choice has a price. 

每一个选择都是有代价的

A good designer understands this tension and minimizes costs by making informed tradeoffs between the needs of the present and the possibilities of the future.

一个好的设计师了解这种紧张关系，并通过在当前的需求和未来的可能性之间做出明智的权衡，将成本降到最低。



## 2.3 Writing Code That Embraces Change

你可以让Gear类更加容易被改变，即使你不知道以后会有什么样的变化。

Because change is inevitable, coding in a changeable style has big future payoffs.

因为变更是不可避免的，以高可变性的风格进行编码，未来会有很大的回报。

As an additional bonus, coding in these styles will improve your code, today, at no extra cost.

作为一个额外的奖励，用这样的方式编程将改善你的代码，就在今天，没有额外的费用。



### 2.3.1 Depend on Behavior, not data

Behavior is captured in methods and invoked by sending messages. 

行为被捕获在方法中，并通过发送消息来调用。当你创建具有单一责任的类时，每一个微小的行为都只存在于一个地方。

When you create classes that have a single responsibility, every tiny bit of behavior lives in one and only one place.

DRY(Don't Repeat Yourself) code tolerates change because any change in behavior can be made by changing code in just one place.

In addition to behavior, objects often contain things you might think of as data. 

除了行为之外，对象还经常包含一些你可能认为是数据的东西。

These data objects are held in instance variables and can be anything from simple strings to complex hashes. 

这些数据对象保存在实例变量中，可以是任何东西，从简单的字符串到复杂的哈希。

Data can be accessed in one of two ways; you can refer directly to the instance variable or you can wrap the instance variable in an accessor method.

数据可以通过两种方式之一被访问；你可以直接引用实例变量，或者你可以将实例变量包裹在访问器方法中。

#### Hide Instance Variables



```ruby
# default-ish implementation via attr_reader
def cog
  @cog
end
```

当代码要做改变时，你调整cog方法里的内容就好了。 `@cog`被包裹在cog方法中。

```ruby
# a more complex one
def cog
  @cog * (foo? ? bar_adjustment : baz_adjustment)
end
```

第二个调整是在方法中做的简单行为改变，但当应用于一堆实例变量引用时，就会造成代码的混乱。



使用attr_reader来包裹Ruby变量会引发两个新问题。

第一个是可见性。可以通过`private` 关键字加以解决。

第二个问题比较抽象。因为可以将每个实例变量包裹在一个方法中，因此可以将任何变量当作另一个对象来对待，数据和普通对象之间的区别开始消失。

虽然有时将应用程序的某些部分仅仅看作是数据是权宜之计，但大多数东西最好被看作是普通的对象。

Regardless of how far your thoughts move in this direction, you should hide data from yourself. 

无论你的想法朝这个方向发展到什么程度，==**你都应该把数据隐藏起来**==。

Doing so protects the code from being affected by unexpected changes. 

这样做可以保护代码不受意外变化的影响。

Data very often has behavior that you don’t yet know about. 

数据很多时候有你还不知道的行为。

Send messages to access variables, even if you think of them as data.

发送消息来访问变量，即使你认为它们是数据。

#### Hide Data Structures

如果说被附在一个实例变量上是坏事，那么依赖复杂的数据结构就更坏了。

bad example:

```ruby
class ObscuringReferences
  attr_reader :data
  def initialize(data)
    @data=data
  end
  def diameters
    # 0 is rim, 1 is tire
    data.collect { |cell|
      cell[0] + (cell[1] * 2)}
  end
end
```

This class certainly does everything necessary to hide the instance variable from itself.

这个类是隐藏了自己的实例变量

但@data有着复杂的数据结构，仅仅隐藏实例变量是不够的。data方法仅仅是返回一个数组

为了做点有用的事，每个data的发送者都必须完全知道数据在数组的哪个索引初。

It depends upon the array’s structure. 

这取决于数组的结构

如果结构改变，那么代码必需改变。

当你在一个数组中拥有数据时，很快就会对数组的结构进行全部的引用。

这些引用很 **==leaky==**

他们规避了封装。

他们不是DRY的

rims在[[0]]为止的知识不应该重复，他应该只在一个地方被知晓。



这个简单的例子已经够糟的了。

如果data返回一个在很多地方都被引用的哈希数组。对其结构的改变会在你的整个代码中层出不穷；每一个改变都代表着一个制造bug的机会， 这个bug使如此隐蔽，以至于你试图找到它都会让你痛哭流涕。

直接引用到复杂的结构中是很混乱的，因为它们掩盖了数据的真正含义，而且它们是维护的噩梦，因为当数组结构改变时，每个引用都需要改变。



```ruby
class RevealingReferences
  attr_reader :wheels
  def initialize(data)
    @wheels = wheelify(data)
  end
  def diameters
    wheels.collect {|wheel|
      wheel.rim + (wheel.tire * 2)}
  end
  
  Wheel = Struct.new(:rim, :tire)
  def wheelify(data)
    data.collect{|cell|
      Wheel.new(cell[0], cell[1])}
  end
end
```

### 2.3.2 Enforce Single Responsibility Everywhere

#### 将额外的职责从方法中提取出来

方法和类一样应该有单一的职责。原因同样，只有一个职责意味着让他容易改变并且易于重用。

bad example:

```ruby
def diameters
  wheels.collect {|wheel|
    wheel.rim + (wheel.tire *2)}
end
```

这个方法明显有两个职责。

1. 遍历wheels
2. 计算每个wheel的直径

将这个方法分解为两个方法，每个只有一个职责。

```ruby
# - calculate diameter of ONE wheel
def diameter(wheel)
  wheel.rim + (wheel.tire *2)
end

# - iterate over the array
def diameters
  wheels.collect {|wheel| diameter(wheel)}
end
```



```ruby
def gear_inches
  ratio * diameter
end
def diameter
  rim + (tire * 2)
end
```

The gear_inches method now sends a message to get wheel diameter. Notice that the refactoring does not alterhow diameter is calculated; it merely isolated the behavior in a separate method.

一旦隔离出来，就会发现Gear的直径方法完全依赖于Wheel中的东西。这表明这个方法应该在Wheel中。



即使你不知道最终的设计是什么，也要做这些重构。

需要这样做的原因不是因为设计明确，相反，恰恰因为设计不明确。

You do not have to know where you’re going to use good design practices to get there.

你不必知道你要去哪使用good design practices来达到目的。

Good practices reveal design.

好的做法揭示了设计。



简单的重构是的问题突显。Gear当然应该负责计算gear_inches，但Gear不应该计算wheel的直径。

单独这个重构的影响看起来不大，但这种编码风格的积累效应非常巨大。



* **Expose previously hidden qualities**
  * 重构一个类以至它所有的方法都有单一职责。即使你今天不打算将这些方法重新组合进其他类，让每个方法都有一个单一的目的，使得该类做的一系列事情更加明显。
* Avoid the need for comments
  * 如果方法中的一些代码需要注释，将他们提取到一个单独的方法当中。新的方法名和之前的注释作用相同。
* Encourage usage
  * 精小的方法鼓励让你应用更健康的编码行为
  * 其他程序员将会重用方法，而不是复制代码
  * 它们会按照你建立的模式，依次创建小的、可重用的方法。这种编码风格会自我传播。
* 易于放置在其他类当中
  * 当你有了更多的设计信息并决定做出改变，精小的方法更易于移动。你可以不用做一堆方法提取和重构就能重新安排行为。
  * 精小方法降低了让你设计进步的门槛



#### Isolate Extra Responsibilities in Classes

一旦每个方法都有了其单一职责，类的范围将会更加明显。

```ruby
class Gear
  attr_reader :chainring, :cog, :wheel
  def initialize(chainring, cog, rim, tire)
    @chainring = chainring
    @cog = cog
    @wheel = Wheel.new(rim, tire)
  end
  def ratio
    chainring / cog.to_f
  end
  def gear_inches
    ratio * wheel.diameter
  end
  
  Wheel = Struct.new(:rim, :tire) do
    def diameter
      rim + (tire * 2)
    end
  end
end

```

## 2.4 Finally, The real wheel

```ruby
class Gear
  attr_reader :chainring, :cog, :wheel
  def initialize(chainring, cog, wheel=nil)
    @chainring	= chainring
    @cog				= cog
    @wheel			= wheel
  end
  
  def ratio
    chainring / cog.to_f
  end
  
  def gear_inches
    ratio * wheel.diameter
  end
end

class Wheel
  attr_reader :rim, :tire
  
  def initialize(rim, tire)
    @rim	= rim
    @tire	= tire
  end
  
  def diameter
    rim + (tire * 2)
  end
  
  def circumference
    diameter * Math::PI
  end
end


```





