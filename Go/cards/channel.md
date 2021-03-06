# Channel

### 通道类型和值

* `chan T`表示一个元素类型为`T`的**双向通道**类型。编译器允许从此类型的值中接收和向此类型的值中发送数据
* `chan<- T`表示一个元素类型为`T`的单向**发送**通道类型。编译器不允许从此类型的值中接收数据
* `<-chan T`表示一个元素类型为`T`的单向**接收**通道类型。编译器不允许向此类型的值中发送数据

双向通道`chan T`可以被隐式转换为单向通道类型`chan<- T`和`<-chan T`，但反之不行。





### 通道操作

1. 调用内置函数`close`来关闭一个通道

   

   ```go
   close(ch)
   ```

   传给`close`函数调用的实参必须为一个通道值，并且此通道**不能为单向接收的**。

   

2. 向通道`ch`发送一个值`v`

```go

ch <- v // ch不能为单向接收通道， <- 称为数据发送操作符

```

3. 从通道`ch`接收一个值

```go

<-ch

```

`ch`不能为单向发送通道。`<-`称为**数据接收**操作符

4. 查询一个通道的容量

```go
cap(ch)
```

其中`cap`是一个已经在[容器类型](https://gfw.go101.org/article/container.html#cap-len)一文中介绍过的内置函数。 `cap`的返回值的类型为内置类型`int`。



5. 查询一个通道的长度：

```go
len(ch)
```

如果被查询的通道为一个nil零值通道，则`cap`和`len`函数调用都返回`0`。 这两个操作是如此简单，所以后面将不再对它们进行详解。 事实上，这两个操作在实践中很少使用。

### 通道操作详解

后文将通道划分为三类

1. 零值（nil）通道
2. 非零值但已关闭的通道
3. 非零值并且尚未关闭的通道

|   操作   | 一个零值nil通道 | 一个非零值但已关闭的通道 | 一个非零值且尚未关闭的通道 |
| :------: | :-------------: | :----------------------: | :------------------------: |
|   关闭   |    产生恐慌     |         产生恐慌         |        成功关闭(C)         |
| 发送数据 |    永久阻塞     |         产生恐慌         |    阻塞或者成功发送(B)     |
| 接收数据 |    永久阻塞     |       永不阻塞(D)        |    阻塞或者成功接收(A)     |

* 关闭一个nil通道或者一个已经关闭的通道将产生一个**恐慌**（`panic`）
* 向一个已关闭的通道发送数据也将导致一个**恐慌** （`panic`)
* 向一个`nil`通道发送数据或者从一个`nil`通道接收数据将使当前协程**永久阻塞**。



我们可以认为一个通道内部维护了三个**队列**（均可被视为先进先出队列）：

1. `接收数据协程队列`（可以看做是**先进先出队列**但其实并不完全是，见下面解释）。此队列是一个没有长度限制的链表。 此队列中的协程均处于`阻塞`状态，<u>它们正等待着从此通道接收数据</u>。
2. `发送数据协程队列`（可以看做是**先进先出队列**但其实并不完全是，见下面解释）。此队列也是一个没有长度限制的链表。 此队列中的协程亦均处于`阻塞`状态，<u>它们正等待着向此通道发送数据</u>。 此队列中的每个协程将要发送的值（或者此值的指针，取决于具体编译器实现）和此协程一起存储在此队列中。
3. `数据缓冲队列`。这是一个循环队列（绝对先进先出），它的长度为此通道的容量。此队列中存放的值的类型都为此通道的元素类型。 如果此队列中当前存放的值的个数已经达到此通道的容量，则我们说此通道已经处于**满槽**状态。 如果此队列中当前存放的值的个数为零，则我们说此通道处于**空槽**状态。 对于一个`非缓冲通道`（容量为零），它总是**同时**处于满槽状态和空槽状态。



每个通道内部维护着一个`互斥锁`用来在各种通道操作中防止数据竞争。



**通道操作情形A**：当一个协程R尝试从一个非零且尚未关闭的通道**接收**数据的时候，此协程`R`将首先尝试获取此通道的锁，成功之后将执行下列步骤，直到其中一个步骤的条件得到满足。

1. 如果此通道的缓冲队列不为空（这种情况下，接收数据协程队列必为空），此协程`R`将从缓冲队列取出（接收）一个值。 如果发送数据协程队列不为空，一个发送协程将从此队列中弹出，此协程欲发送的值将被推入缓冲队列。此发送协程将恢复至运行状态。 接收数据协程`R`继续运行，不会阻塞。对于这种情况，此数据接收操作为一个**非阻塞操作**。
2. 否则（即此通道的缓冲队列为空），如果发送数据协程队列不为空（这种情况下，此通道必为一个非缓冲通道）， 一个发送数据协程将从此队列中弹出，此协程欲发送的值将被接收数据协程`R`接收。此发送协程将恢复至运行状态。 接收数据协程`R`继续运行，不会阻塞。对于这种情况，此数据接收操作为一个**非阻塞操作**。
3. 对于剩下的情况（即此通道的缓冲队列和发送数据协程队列均为空），此接收数据协程`R`将被推入接收数据协程队列，并进入阻塞状态。 它以后可能会被另一个发送数据协程唤醒而恢复运行。 对于这种情况，此数据接收操作为一个**阻塞操作**。
   * 第三种情况，发送方和缓冲队列都没有东西，接收方就**阻塞**在那里了

**通道操作情形B**： 当一个协程`S`尝试向一个非零且尚未关闭的通道**发送**数据的时候

，此协程`S`将首先尝试获取此通道的锁，成功之后将执行下列步骤，直到其中一个步骤的条件得到满足。

1. 如果此通道的`接收数据协程队列`不为空（这种情况下，缓冲队列必为空）， 一个接收数据协程将从此队列中弹出，此协程将接收到发送协程`S`发送的值。此接收协程将恢复至运行状态。 发送数据协程`S`继续运行，不会阻塞。对于这种情况，此数据发送操作为一个**非阻塞操作**。
2. 否则（接收数据协程队列为空），如果缓冲队列未满（这种情况下，发送数据协程队列必为空）， 发送协程`S`欲发送的值将被推入缓冲队列，发送数据协程`S`继续运行，不会阻塞。 对于这种情况，此数据发送操作为一个**非阻塞操作**。
3. 对于剩下的情况（接收数据协程队列为空，并且缓冲队列已满），此发送协程`S`将被推入发送数据协程队列，并进入阻塞状态。 它以后可能会被另一个接收数据协程唤醒而恢复运行。 对于这种情况，此数据发送操作为一个**阻塞操作**。



**通道操作情形C**： 当一个协程成功获取到一个非零且尚未关闭的通道的锁并且准备关闭此通道时

，下面两步将依次执行：

1. 如果此通道的接收数据协程队列不为空（这种情况下，缓冲队列必为空），此队列中的所有协程将被依个弹出，并且每个协程将接收到此通道的元素类型的一个零值，然后恢复至运行状态。
2. 如果此通道的发送数据协程队列不为空，此队列中的所有协程将被依个弹出，并且每个协程中都将产生一个恐慌（因为向已关闭的通道发送数据）。 这就是我们在上面说并发地关闭一个通道和向此通道发送数据这种情形属于不良设计的原因。 事实上，在数据竞争侦测编译选项（`-race`）打开时，Go官方标准运行时将很可能会对并发地关闭一个通道和向此通道发送数据这种情形报告成数据竞争。

注意：当一个缓冲队列不为空的通道被关闭之后，它的缓冲队列不会被清空，其中的数据仍然可以被后续的数据接收操作所接收到。详见下面的对情形D的解释。



**通道操作情形D**： 一个非零通道被关闭之后，此通道上的后续数据接收操作将永不会阻塞。 此通道的缓冲队列中存储数据仍然可以被接收出来。 伴随着这些接收出来的缓冲数据的第二个可选返回（类型不确定布尔）值仍然是`true`。 一旦此缓冲队列变为空，后续的数据接收操作将永不阻塞并且总会返回此通道的元素类型的零值和值为`false`的第二个可选返回结果。 上面已经提到了，一个接收操作的第二个可选返回（类型不确定布尔）结果表示一个接收到的值是否是在此通道被关闭之前发送的。 如果此返回值为`false`，则第一个返回值必然是一个此通道的元素类型的零值。



- 如果一个通道已经关闭了，则它的发送数据协程队列和接收数据协程队列肯定都为空，但是它的缓冲队列可能不为空。
- 在任何时刻，如果缓冲队列不为空，则接收数据协程队列必为空。
- 在任何时刻，如果缓冲队列未满，则发送数据协程队列必为空。
- 如果一个通道是缓冲的，则在任何时刻，它的发送数据协程队列和接收数据协程队列之一必为空。
- 如果一个通道是非缓冲的，则在任何时刻，一般说来，它的发送数据协程队列和接收数据协程队列之一必为空， 但是有一个例外：一个协程可能在一个[`select`流程控制](https://gfw.go101.org/article/channel.html#select)中同时被推入到此通道的发送数据协程队列和接收数据协程队列中。



### 通道的元素值的传递都是复制过程

和赋值以及函数调用传参一样，当一个值被传递时，只有它的直接部分被复制

对于官方标准编译器，最大支持的通道的元素类型的尺寸为`65535`。 但是，一般说来，为了在数据传递过程中`避免过大的复制成本`，我们不应该使用尺寸很大的通道元素类型。 如果欲传送的值的尺寸较大，应该改用`指针类型`做为通道的元素类型。



### 关于通道和协程的垃圾回收

注意，一个通道被其发送数据协程队列和接收数据协程队列中的所有协程引用着。因此，如果一个通道的这两个队列只要有一个不为空，则此通道肯定不会被垃圾回收。 另一方面，如果一个协程处于一个通道的某个协程队列之中，则此协程也肯定不会被垃圾回收，即使此通道仅被此协程所引用。 事实上，一个协程只有在退出后才能被垃圾回收。



### 数据接收和发送操作都属于简单语句

数据接收和发送操作都属于[简单语句](https://gfw.go101.org/article/expressions-and-statements.html#simple-statements)。 另外一个数据接收操作总是可以被用做一个单值表达式。 简单语句和表达式可以被用在[一些控制流程](https://gfw.go101.org/article/control-flows.html)的某些部分。

在下面这个例子中，数据接收和发送操作被用在两个`for`循环的初始化和步尾语句。





```go

for v:=range aChannel {
  // 使用v
}

// 等价于

for {
  v, ok = <-aChannel
  if !ok {
    break
  }
}


```

### `select-case`分支流程控制代码块

Go中有一个专门为通道设计的`select-case`分支流程控制语法。 此语法和`switch-case`分支流程控制语法很相似。 比如，`select-case`流程控制代码块中也可以有若干`case`分支和最多一个`default`分支。 但是，这两种流程控制也有很多不同点。在一个`select-case`流程控制中，

- `select`关键字和`{`之间不允许存在任何表达式和语句。
- `fallthrough`语句不能被使用.
- 每个`case`关键字后必须跟随一个通道接收数据操作或者一个通道发送数据操作。 通道接收数据操作可以做为源值出现在一条简单赋值语句中。 以后，一个`case`关键字后跟随的通道操作将被称为一个`case`操作。
- 所有的非阻塞`case`操作中将有一个被随机选择执行（而不是按照从上到下的顺序），然后执行此操作对应的`case`分支代码块。
- 在所有的`case`操作均为阻塞的情况下，如果`default`分支存在，则`default`分支代码块将得到执行； 否则，当前协程将被推入所有阻塞操作中相关的通道的发送数据协程队列或者接收数据协程队列中，并进入阻塞状态。

==一个不含任何分支的`select-case`代码块`select{}`将使得当前协程处于永久阻塞状态==

```go
package main

import "fmt"

func main() {
	var c chan struct{} // nil
	select {
	case <-c:             // 阻塞操作
	case c <- struct{}{}: // 阻塞操作
	default:
		fmt.Println("Go here.")
	}
}
```



## 通道操作事例

### 采用最快回应

本用例可以看作是上例中只使用一个通道变种的增强。

有时候，一份数据可能同时从多个数据源获取。这些数据源将返回相同的数据。 因为各种因素，这些数据源的回应速度参差不一，甚至某个特定数据源的多次回应速度之间也可能相差很大。 同时从多个数据源获取一份相同的数据可以有效保障低延迟。我们只需采用最快的回应并舍弃其它较慢回应。

注意：如果有*N*个数据源，为了防止被舍弃的回应对应的协程永久阻塞，则传输数据用的通道必须为一个容量至少为*N-1*的缓冲通道。

```go

package main

import (
	"fmt"
	"time"
	"math/rand"
)

func source(c chan<- int32) {
	ra, rb := rand.Int31(), rand.Intn(3) + 1
	// 睡眠1秒/2秒/3秒
	time.Sleep(time.Duration(rb) * time.Second)
	c <- ra
}

func main() {
	rand.Seed(time.Now().UnixNano())

	startTime := time.Now()
	c := make(chan int32, 5) // 必须用一个缓冲通道
	for i := 0; i < cap(c); i++ {
		go source(c)
	}
	rnd := <- c // 只有第一个回应被使用了
	fmt.Println(time.Since(startTime))
	fmt.Println(rnd)
}

```

### 使用通道实现通知

**通知**可以被看作是特殊的**请求/回应用例**。在一个通知用例中，我们并不关心回应的值，我们只关心回应是否已发生。 所以我们常常使用空结构体类型`struct{}`来做为通道的元素类型，因为空结构体类型的尺寸为零，能够节省一些内存（虽然常常很少量）。

#### 向一个通道发送一个值来实现单对单通知

我们已知道，如果一个通道中无值可接收，则此通道上的下一个接收操作将阻塞到另一个协程发送一个值到此通道为止。所以一个协程可以向此通道发送一个值来通知另一个等待着从此通道接收数据的协程。

在下面这个例子中，通道`done`被用来作为一个信号通道来实现单对单通知。

```go

package main

import (
	"crypto/rand"
	"fmt"
	"os"
	"sort"
)

func main() {
	values := make([]byte, 32 * 1024 * 1024)
	if _, err := rand.Read(values); err != nil {
		fmt.Println(err)
		os.Exit(1)
	}

	done := make(chan struct{}) // 也可以是缓冲的

	// 排序协程
	go func() {
		sort.Slice(values, func(i, j int) bool {
			return values[i] < values[j]
		})
		done <- struct{}{} // 通知排序已完成
	}()

	// 并发地做一些其它事情...

	<- done // 等待通知
	fmt.Println(values[0], values[len(values)-1])
}

```





















