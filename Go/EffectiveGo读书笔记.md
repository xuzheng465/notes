# 数据 Data

Go提供了两种allocation元语，即内置函数`new`和`make`。

new：

* 用来分配内存。
* 不会初始化内存，只会将内存置零。
* new(T) 会为类型为 T 的新项分配已置零的内存空间， 并返回它的地址，也就是一个类型为 `*T` 的值。
* 用 Go 的术语来说，它返回一个指针， 该指针指向新分配的，类型为 T 的零值。



既然new返回的内存已置零，那么当你设计数据结构时，每种类型的零值就不必进一步初始化了。这意味着该数据结构的使用者只需用new创建一个新的对象就能工作。

例如， bytes.Buffer的文档中提到零值的Buffer就是已准备就绪的缓冲区。同样sync.Mutex并没有显式的构造函数或init方法，零值的sync.Mutext就已经定义为已解锁的互斥锁了。零值属性是**传递**的。

```go
type SyncedBuffer struct {
  lock sync.Mutex
  buffer bytes.Buffer
}
p := new(SyncedBuffer) // type *SyncedBuffer
var v SyncedBuffer			// type SyncedBuffer
```



