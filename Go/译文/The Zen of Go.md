https://dave.cheney.net/2020/02/23/the-zen-of-go

半机翻

# The Zen of Go

[Dave Cheney](https://dave.cheney.net/)

## 应该如何写出好的代码?



（正文开始部分 [the zen of ~~Python~~ Go](##The Zen of ~~Python~~ Go))

当我反思我自己的工作时，经常在思考一个问题，应该如何写好代码？这也是个常见的副标题题目。鉴于没有人会主动去写烂代码，这就引出一个问题：你怎么知道你自己什么时候写了好的Go代码.

如果好与坏之间有一个Continuum, 我们怎么知道好的部分是什么? 它的特性, 属性, 标志, 模式, idioms是什么?

## Idiomatic Go

<img src="./The Zen of Go.assets/1011226.jpg" alt="img" style="zoom:70%;" />

这就要说到idiomatic Go了。要说某件事时idiomatic，是说它遵循了当时的风格. 如果某事不是idiomatic，那就是不符合当时的风格。它是不时尚的。

更重要的是，对别人说他们的代码不够idiomatc，并不能解释为什么不够idiomatic。为什么会这样？像所有真理一样，我们都能在字典中找到答案。

> *idiom (noun): a group of words established by usage as having a meaning not deducible from those of the individual words.*

（翻译成成语，或者习语的话: 汉语词汇中特有的一种长期相沿习用的固定短语。来自于古代经典或著名著作历史故事和人们的口头,意思精辟,往往隐含于字面意义之中,不是其构成成分意义的简单相加,具有意义的整体性。它结构紧密,一般不能任意变动词序,抽换或增减其中的成分,具有结构的凝固性.[🔗](http://www.zdic.net/hans/%E6%88%90%E8%AF%AD))

Idioms 是共同价值观的标志。Idiomatic Go不是从书本上学来的，而是通过融入社区获得的。

<img src="./The Zen of Go.assets/mean-girls-you-cant-sit-with-us-main.jpg" alt="img" style="zoom:70%;" />



我对idiomatc Go最为担心的是，在很多方面，它会是排他性的。这是在说“你不能和我们坐在一起！”这不正是批评其他人代码不够idiomatic时的意思吗？他们做得不对，它看起来不对。它不符合当前的风格。

我认为idiomatic Go并不是一个适合的教人们写出好Go代码的机制。因为，从根本上来说，它就是告诉别人他们做错了。如果我们的建议能让他们在最愿意接受的时候就采纳，那不是更好吗？



## Proverbs

撇开有点问题的idioms不谈，Gopher们还有哪些文化产品(?cultural artefacts)? 或许我们可以转向Rob Pike的Go Proverbs（谚语）。这些是适合的教学工具吗？这些能告诉新手如何写好Go代码吗？

一般来讲，我不这么认为。这并不是要否定Pike的工作成果。它只是Go谚语，和濑户健作的原著一样。都是观察，而不是价值陈述。再一次，词典来救场。

> *proverb (noun): a short, well-known pithy saying, stating a general truth or piece of advice.*
>
> 谚语：民间流传的简练通俗而富有意义的语句[🔗](http://www.zdic.net/hans/%E8%B0%9A%E8%AF%AD)



Go Proverbs的目的是为了揭示语言设计的深层真相。但如***empty interface says nothing*** 这样的的话对一个来自没有结构类型的语言的新手来说， 又有多大用处呢？

必须认识到，在一个不断发展的社区里，任何时候，新手的数量远远超过哪些自称掌握了Go这门语言的人。因此，在这种情况下，谚语也不见得是最好的教学工具。



## Engineering Values

Dan Luu找到了Mark Lucovsky关于windows NT-windows 2000时间段前后windows团队的工程文化的一个旧演示。我之所以提到它，是因为Lukovsky将文化描述为一种评估设计和做出权衡的普遍方式。(The reason I mention it is Lukovsky’s description of a culture as a common way of evaluating designs and making tradeoffs.)



<img src="./The Zen of Go.assets/Lucovsky.001.jpeg" alt="img" style="zoom:80%;" />



讨论文化的方式有很多，但就工程文化而言，Lucovsky的描述还是很合适的。其中心思想是***在一个未知的设计空间中，价值观指导决策***。（The central idea is *values guide decisions in an unknown design space*.）NT团队的价值观是：可移植性、可靠性、安全性和可扩展性。工程价值观，粗略地翻译一下，就是**这里的做事方式**。



## Go's Values



Go显式的价值观是什么？基于什么核心信念或哲学，定义了Go程序员解释世界的方式？他们是如何被传播的？是如何被教授的？是如何被执行的？它们是如何随着时间的推移而变化的？



作为一个新晋的Go程序员， 你将如何灌输Go的工程价值？或者，你作为一个经验丰富的Go专家如何将这些价值传播给下一代的Go程序员们。



## The values of other languages



为了衬托我所要表达的意思，我们可以看看其他语言，看看他们的工程价值。



例如，C++（以及延伸到Rust）认为程序员不应该为他们不使用的功能付费。如果一个程序没有使用一些计算代价昂贵的语言特性，那么这个程序就不应该被迫承担该特性带来的成本。这个价值观从语言衍生到标准库，并作为判断所有用C++所编写代码的设计尺度。



在Java、Ruby和Smalltalk中，万物皆对象的核心价值驱动着围绕消息传递、信息隐藏和多态的程序设计。在这些语言中硬塞进程序化风格，甚至是函数化风格的设计，被认为是错误的--或者像Gophers所说的那样，是non-idiomatic。



对我们自己的社区来说，将Go程序员联系在一起的工程价值观是什么。在我们的社区中，讨论往往是分裂的，所以从第一原则中得出一套价值观是一个巨大的挑战。达成共识是至关重要的，但随着参与讨论的人越来越多，达成共识的难度就会成倍增加。但如果有人为我们做了这些苦活呢？



## The Zen of ~~Python~~ Go

几十年前(??), Tim Peters坐下了写了PEP-20，the Zen of Python。Peters试图记录他看到Guido van Rossum在担任Python的BDFL（仁慈的终身独裁者）时应用的工程价值观。



在本文余下的时间里，我要看向Python之禅，看看有没有什么可以为Go程序员的工程价值观提供参考？



### A good package starts with a good name



Let’s start with something spicy.

> *“Namespaces are one honking great idea–let’s do more of those!”*
>
> The Zen of Python, Item 19

这一条非常明确， Python程序员应该多多使用命名空间。

在Go术语中，命名空间就是一个package。对于将程序归类成package有利于设计和潜在的重用这一观点，应该不会有任何问题。但可能会有一些困惑，特别你是有着十年其他语言经验的的老程序员，可能并不知道正确的做法是什么。

在Go中，每个包应该有**一个**目的。而了解其目的的最好方法就是看包的名字，一个名词。包的名字描述了他所提供的功能。所以重新解释Peters的话，每个Go的包都应该只有一个目的。

这不是一个新概念，我已经说过有一阵了。为什么你要这样做，而不是使用包来进行细粒度分类的方法？因为“变化”。

> Design is the art of arranging code to work today, and be changable forever.
>
> ​																										-- Sandi Metz

变化是我们所玩游戏的名称。作为程序员，我们要做的就是管理变化。当我们将这事做的好时，我们称之为“设计”， 或者架构。当我们做的不好时，我们将之称为技术债务，或者说是遗留代码（~~祖传屎坑~~)



如果你写了一个程序，对于一组固定的输入，一次工作得非常完美。那么没人关心代码好坏，因为最终程序的输出是企业关心的全部。



但事情并不总是这样。软件有bug，需求变更，输入变更。没有什么程序只要求运行一次， 因此你的程序将随着时间变化。或许是你将要应对这个，更可能是其他人。但肯定是有人要去变更代码，肯定是要有人维护代码。



我们怎么做才能让程序更易于改变呢？ Interfaces everywhere? Make everything mockable? Pernicious dependency injection? 也许对某些类别的程序这些技术是有用的，但不是很多程序都能这样。然而，对大多许程序来说前期设计的程序灵活性高有些过度工程。



假如我们做的是不是增强组建，而是替换他们。那么其需要替换的最佳时机，就是当它不像纸上写的那样。



一个好的package首先要选一个好名字。把你的包名当作是一个电梯广告。只用一个词来描述他所提供的功能。当名字不再符合要求时，就找一个替代品。

### Simplicity matters

> Simple is better than complex
>
> The Zen of Python, Item 3



> “There are two ways of constructing a software design: One way is to make it so simple that there are obviously no deficiencies, and the other way is to make it so complicated that there are no obvious deficiencies. The first method is far more difficult.”
>
> C. A. R. Hoare, The Emperor’s Old Clothes, 1980 Turing Award Lecture

明显没有错误， 没有明显的错误。





