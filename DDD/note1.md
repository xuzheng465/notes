# Modern Software Architecture: Domain Models, CQRS, and Event Sourcing



Big Ball of Mud (BBM)

A system that's largely unstructured, padded with **hidden dependencies** between parts, with a lot of data and code duplication and an unclear identification of layers and concerns - a spaghetti code jungle.

Yummy



### Why DDD so intriguing

* Captured known elements of desgin process
* Organized them into a set of principles
* Made **domain modeling** the focus of development
* **Different** approach to building business logic

### DDD is Still about business Logic

1. Crunch knowledge about the domain
2. Recognize subdomains
3. Design a Rich domain model
4. Code by telling objects in the domain model what to do

### Supreme Goal

Tackling Complexity in the Heart of Software

* Wonderful idea
* Not a mere promise
* Not really hard to do right
* But just easier to do wrong





Domain Model remains a valid pattern to organize the business logic but other patterns can be used as well:	

* Object-oriented models
* Functional models
* CQRS
* Classic 3-tier
* 2-tier



## Ubiquitous language

“Use the model as the backbone of a language"

Eric Evans

* Discovering the ubiquitous language
* leads you to understand the business domain
* in order to design a model

PS: Any model that works, not necessarily oo model



Words and verbs that truly reflect the semantics of the business domain



:exclamation:**Different** concepts named differently.

:exclamation:**Matching** concepts named equally.

<img src="./note1.assets/image-20201212105042634.png" alt="image-20201212105042634" style="width:600px;" />



<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212105152227.png" alt="image-20201212105152227" style="width:600px" />

Missing a point is creating a bug.



## Bounded Context



<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212111336970.png" alt="image-20201212111336970" style="zoom:50%;" />

Not duplication, it's clarification.

Forcing abstraction, thus having different functionalities full under the umbrella of the same class, that would likely be a violation of the **==single resoponsibility principle==**.



Context map is the diagram that provides a comprehensive view of the system being designed.

## Context map

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212112019500.png" alt="image-20201212112019500" style="width:600px;" />

**Upstream** context influences **Downstream** context.

U influnces D, but vice versa is not true.

Influences may take various forms. For sure it means the code in the upstream context is available as a reference to the downstream.

It also means the schedule of work in Upstream context cannot be changed and won't be changed from within the downstream. context.

但同时Upstream context背后的团队对变更请求的响应速度也可能不尽如人意。

### Relationships

* Conformist(墨守成规的人;循规蹈矩的人):
  * Downstream Context depends on upstream context.
  * No negotiation possible
* Customer/Supplier:
  * Customer context depends on supplier context
  * Chance to raise concerns and have them addressed in some ways.
* Partner:
  * Mutual dependency between the two contexts
* Shared Kernel
  * Shared Model that can't be changed without consulting teams in charge of contexts that depend on it.
* Anti-corruption layer
  * Additional layer giving the downstream context a fixed interface no matter what happens in the upstream context.

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212115704241.png" alt="image-20201212115704241" style="width:600px;" />





<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212120023240.png" alt="image-20201212120023240" style="width:600px;" />



首先，它代表了一种快速获得业务领域全面视野的方法。

事件风暴的一个有价值的输出是在每个上下文中发现的有界上下文和聚合体的列表。

这里的聚合体本质上是处理命令和事件并控制持久性的软件组件。

事件风暴还有助于形成关于系统中用户类型的想法，以及哪里是用户体验特别重要的地方，甚至比实际执行的任务更重要。

在这种情况下，从用户体验角度出发的关键屏幕的草图可以添加到事件风暴的输出中。

事件暴击是一种令人惊喜的体验，有时甚至是一种震撼人心的体验，但它是一个新事物，所以你可能无法在外面找到详细的说明，至少没有你所期望的数量。

## Module 3 The DDD Layered Architecture

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212120859425.png" alt="image-20201212120859425" style="width:600px;" />



<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212121739886.png" alt="image-20201212121739886" style="width:600px;" />

**Layer**

the term layer should be used to identify a logical container for a portion of code

**Tier**

The term tier indicate some physical containers for code. 

经典三层

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212123736561.png" alt="image-20201212123736561" style="width:600px;" />

物理上往往两层。

Domain-driven design brought to a slight variation of the classic 3-tier model. 

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212124718578.png" alt="image-20201212124718578" style="width:600px;" />

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212124804051.png" alt="image-20201212124804051" style="width:600px;" />

### Presentation Layer

Responsible for providing the **user interface** to accomplish any required tasks.

Responsible for providing an effective, smooth, and even pleasant **user experience**.



User experience is then defined as the experience the user goes through when she interacts with the application screens.



* Task-based nature
* Device-friendly
* User-friendly
* Faithful to real-world process

### Application Layer

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212125902046.png" alt="image-20201212125902046" style="width:600px;" />





### Business Logic



#### Business Rule

Statements that detail the implementation of a buisness process or describe a business policy to be taken into account.







#### Domain Model

领域模型模式的要点是 hitting an object-oriented model, 打击面向对象模型，其完全代表业务领域的行为和流程。在实现这个模式时，你有代表领域中entities的的类。

这些累暴露了属性和方法，而方法指的是entity(实体)的实际行为和业务规则。

聚合模型是领域驱动设计中的一个术语，指的是领域模型的核心对象。

领域模型中的类对持久化来讲应该是不可知的，并且与服务类配对。这些服务类只包含将类的实例物化到持久化层和从持久化层出来的逻辑。

领域模型的图形模式有两个元素，一个是聚合对象的模型，另一个是服务，用来执行跨越多个聚合对象或直接处理持久化的具体工作流。

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212160639857.png" alt="image-20201212160639857" style="width:600px;" />

### The Domain Layer

架构师将所有对用例不变的逻辑放置在领域层。

这意味着一个业务领域的软件模型，只是领域模型，以及相关的、互补的一套领域专用服务。

#### Models for the business domain

1. Object-oriented entity model
2. Functional model

#### Guidelines for classes in an entity model

An entity model has two main characteristics. 

Classes in first place follow strict **DDD conventions**, which means that for the most part these classes are expected not to have constructors, but factories, use value types over primitive types, and avoid private setters on properties. 

遵循DDD惯例，在大多数情况下，这些类预计不会有构造函数，而是有工厂。使用值类型（value type）而不是基本类型（primitive type）

> A value type is *usually* whatever type reside on the Stack. A primitive type is a type defined at the programming language level, often it is even a value type, directly supported by the compiler of the language. However this is a summary general answer because each programming language have different set of differences between the two types ...

In addition, these classes are expected to expose both **data and behavior**. 

#### Anaemic Model 贫血型模型

A anemic class just contains data 

the implementation of workflows and business rules is moved to external components, such as domain services.

* Plain **data containers**
* Behavior and rules **moved** to domain services

#### Domain Services

领域服务（Domain Services）是对领域模型的补充，包含那些不适合其他实体的领域逻辑。

这基本上涵盖了两种情况



## Module 4 The Domain Model Supporting Architecture

### Holistic Model for the Bussiness model



<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212170552529.png" alt="image-20201212170552529" style="width:600px;" />



<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212170754073.png" alt="image-20201212170754073" style="zoom:50%;" />

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212170814544.png" alt="image-20201212170814544" style="zoom:50%;" />



在领域驱动设计中，identification and mapping of bounded context 是最重要的。

最后，领域层是业务领域的API，你应该确保不可能对API进行错误的调用，从而破坏领域的完整性。

应该清楚的是，为了与业务保持持续一致，你应该将设计的重点放在行为上，而不是数据上。

要想让领域驱动的设计成功，你必须理解业务领域的工作原理，并用软件来渲染它。

这就是为什么要关注行为的原因。

### Aspects of a domain model

* Domain Model:
  * Modules
    * Entities
    * Values
* Domain Services:
  * Repositories:
    * Provide access to storage
  * Proxies:
    * Towards external web services



#### DDD value types

* Coolection of individual values
* Fully defined by its collection of attributes
* Immutable
* More precise and accurate than primitive types.

Domain logic goes in the domain layer, model or services.

Persistence goes in the infrastructure layer managed by domain services.

#### DDD Entities

* Need an identity
* Uniqueness is important to the specific object
* Typically made of data and behavior
* Contain domain logic, but not persistence logic

#### DDD Aggregates

* A few individual entities constantly used and referenced together
  * 一个Aggreagte是一个逻辑上相关的实体和值类型的集合。
* Cluster of associated objects treated as one for data changes.
  * 相关对象的集群，有数据变化时，将他们视为一个单一的实体
* Aggregate roots
  * 在集群中，有一个根对象（root object）是外界用来查询或命令的公共端点。对聚合体其他成员的访问。总是由聚合体的根来作中介。
* Preserve transactional integrity
  * 两个聚合体之间由某种一致性边界隔开，该边界提示如何将实体分组。
  * 聚合体的设计受到系统所需的业务交易的启发。
  * 这里的一致性指事务上的一致性。
  * 集合中的对象暴露了一个与业务流程完全一致的整体行为

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201212190651540.png" alt="image-20201212190651540" style="zoom:50%;" />



Persistence vs. Domain Model

// TODO

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201213082210457.png" alt="image-20201213082210457" style="width:600px;" />

### That Crazy Little thing Called Behavior

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201213082847670.png" alt="image-20201213082847670" style="zoom:50%;" />



行为具体来说就是验证对象状态的方法
它也是调用要在对象上执行的业务操作的方法
表达涉及对象的业务流程的方法

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201213083635910.png" alt="image-20201213083635910" style="zoom:50%;" />

更好的方案是，当你创建这样的东西时，访问数据的唯一方式是由API中介的，而API是由对象本身暴露的。

所以**API是模型的一部分**。
它不是外部的一层代码，你可以调用也可以不调用。
业务逻辑的容器不是外部的组件，而是作为重点的业务逻辑就内置在对象中。
所以你不是设置属性，而是调用方法，并因为你调用的操作而改变对象的状态。
这是一种更有效的处理和模拟真实世界的方式。

### Aggregates and Value Types

Work with fewer objects and coarse grained and with fewer relationships.

* Protect as much as possible the graph of entities from outsider access
* Ensure the state of child entities is always consistent
* Actual boundaries of aggregates are determinded by business rules

#### Common Responsibilities associated with an aggregate Root

* Ensure encapsulated objects are always in a consistent state
* Take care of persistence for all encapsulated objects
* Cascade updates and deletions through the encapsulated objects.
  * 它必须在图中进行级联更新和删除
* Access to encapsulated objects must always happen by navigation
  * 根必须保证对封装对象的访问始终是中介的，并且只通过根的导航进行。

* 

### Domain Services

* Implement the domain logic that doesn't belong to a particular aggregate and most likely span over multiple entities.(实现领域逻辑，其不属于一个集合体，很可能跨越多个实体的领域逻辑)
* Coordinate the activity of aggregates and repositories with the purpose of implementing a business action. (领域服务协调各个聚合体和存储库的运动，目的是实现所有业务操作，领域服务可能会消耗基础设施的服务，例如当他们需要发送电子邮件或短信时)
* May consume services from the infrastructure, such as when sending an email or a text message is necessary.领域服务可能会消耗基础设施的服务，例如当他们需要发送电子邮件或短信时

Actions in domain services come from requirements and are approved by domain experts.

Names used in domain services are strictly part of the ubiquitous language.

#### Example

##### Determine whether a given customer reached the status of "gold" customer

A customer earns the status of **"gold"** after she exceeds a given threshold of orders on a selected range of products.

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201213093658765.png" alt="image-20201213093658765" style="zoom:50%;" />



##### Booking a meeting room

Booking requires verifying availability of the room and processing payment

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201213093754003.png" alt="image-20201213093754003" style="zoom:50%;" />

### Repositories

In DDD, a repository is just the class that handles **persistence** on behalf of entities and ideally aggreagte roots.

* Most popular type of domain service
* Take care of persisting aggregates
* One repository per aggregate root



* Assembly with repositories has a direct dependency on data stores.
* A repository is where you deal with connection strings and use SQL commands.

### Events in the Business Domain

Events是可选的，但它们只是一种**更有效**和**更有弹性的**方式来表达有时一些现实世界的业务领域的复杂性。

Scenario:

在网店应用中，下了一个订单，系统处理成功，说明付款没问题。
送货单通过了，发货公司也收到了，然后订单生成并录入系统。
现在怎么办呢？假设业务需求希望你在订单生成后执行一些特殊任务。
现在的问题是，你会在哪里实现这样的任务？

**第一种**方案只是将实现额外任务的代码并入执行订单处理的领域服务方法
你基本上是通过必要的步骤来完成结账过程，如果你成功了，那么，在这一点上，你就会执行任何额外的任务。
这一切都同步发生，而且只在一个地方编码。但，它的表现力并不强。
它基本上是单体式（monolithic）代码，如果未来需要更改，你需要触及服务的代码来实现更改，有可能使域服务方法变得相当长，甚至复杂。这更有可能违反**ubiquitous language**。

副词，when，通常指的是观察到企业中的某一事件和采取的行动。

**那么事件呢？**事件让你不必把代码放在同一个地方，同时也带来了其他一些非同小可的好处。
提出事件这个动作与处理事件的动作是有区别的，比如说，处理事件的动作可能有利于测试性。
其次，你可以很容易地拥有多个处理程序来独立处理同一个事件。

<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201213100816487.png" alt="image-20201213100816487" style="zoom:50%;" />







<img src="/Users/xuzheng/Projects/notes/DDD/note1.assets/image-20201213101020553.png" alt="image-20201213101020553" style="width:600px;" />

Events help significantly to coordinate actions within a workflow and to make use-case workflows a lot more resilient to changes.











## CQRS





## Event Sourcing (事件溯源)

