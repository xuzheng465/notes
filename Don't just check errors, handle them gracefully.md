# Don't just check errors, handle them gracefully





## Errors 只是值

我花了很多时间来思考在 Go 程序中处理错误的最佳方式。我真的想要有一个处理错误的单一方式。可以通过死记硬背的方式教给所有Go程序员，就像我们教数学或字母表一样。

然而，我得出的结论是，不存在处理错误的单一方法。相反，我认为Go的错误处理可以分为三个核心策略。



## Sentinel errors

第一种策略是Sentinel errors。

```go
if err == ErrSomething { … }
```

这个名字来源于计算机编程中使用特定的值来表示不会进一步处理的做法。对Go程序来说，我们用特定的值来表示错误。

这种error的例子有`io.EOF` 或者底层错误， 相似syscall包中的常量， `syscall.ENOENT`。

这是最不灵活的错误处理策略了。因为**调用者必须使用等价操作符将结果与预先声明的值进行比较**。当你想要提供更多的上下文时，这就会是个问题， 因为返回不同的错误将会打破相等检查。

即使是使用fmt.Errorf为错误添加一些上下文， 也会使得相等测试失效。取而代之的是，调用者将被迫查看错误的Error 方法的输出， 看看它是否匹配一个特定的字符串。

### 永远不要去查看 `error.Error` 的输出

我认为你永远不应该检查 `error.Error` 方法的输出。error接口上的Error方法是为程序员而存在的，而不是为了程序。

那个输出的内容属于日志文件或是显示在屏幕上。你不应该尝试通过检查其内容去改变你程序的行为。

我知道有时候这是不可能的，并且正如twitter上一些人指出的那样，写测试时根本无法遵守这个意见。尽管如此，在我看来，比较错误的字符串形式不咋好，你应该尽量规避。

### Sentinel errors 作为公开API

如果你的公共函数或方法返回一个特定值的错误，那么这个值必须是公开的，当然也要有文档。这将增加你的API的表面积。

如果你的API定义了一个返回特定错误的接口，那么该接口的所有实现都将被限制为只返回该错误，即使他们可以提供一个更具描述性的错误。

我们在 `io.Reader` 中看到了这一点。像 `io.Copy` 这样的函数，需要阅读器实现准确地返回 `io.EOF` 来向调用者发出不再有数据的信号，但这并不是一个错误。signal， not error



### Sentinel errors 在两个包之间建立了依赖

sentinel errors最严重的问题时在不同的包间建立了依赖。举个例子，要检查错误是否等于io.EOF，你的代码必须导入io包。

这个例子听起来不是那么糟糕，因为它太常见了。 但想象下，当你的项目中的许多包导出错误值时，耦合就产生了，你的项目中的其他包必须导入这些错误值来检查特定的错误条件。

我曾参与过使用这种模式的大型项目，我可以告诉你，不良设计的幽灵--循环导入--从未远离我们的脑子。



### 结论： 不要使用sentinel errors

所以，我的建议是避免在你写的代码中使用哨兵错误值。有一些情况下，它们会在标准库中使用，但这不是你应该效仿的模式。

如果有人要求你从你的包中导出一个错误值，你应该礼貌地拒绝，而是建议一种替代的方法，比如我接下来要讨论的方法。



## Error types

错误类型是我想讨论的第二种Go中错误处理的方式。

```go
if err, ok:= err.(SomeType); ok {}
```

错误类型就是你创建的实现了error interface的类型。在这个例子中，MyError类型追踪文件和行数，还有Msg描述发生了什么。

```go
type MyError struct {
  Msg string
  File string
  Line int
}

func (e *MyError) Error() string {
  return fmt.Sprintf("%s:%d: %s", e.File, e.Line, e.Msg)
}

return &MyError{"Something Happened", "sever.go", 42}
```

因为MyError是一个类型， 调用者可以使用类型断言从错误中提取额外的上下文。

```go
err := something()
switch err:=err.(type) {
case nil:
  // call succeeded, nothing to do
case *MyError:
  fmt.Println("error occurred on line:", err.Line)
default:
  // unknown error
  return err
}
```

与错误值相比，错误类型的一大改进时他们能够包裹底层错误以提供更多的上下文。

一个绝佳的例子就是os.PathError类型，它将要执行的操作和文件路径都记录在类型里。

```go
// PathError records an error and the operation
// and file path that caused it.
type PathError struct {
        Op   string
        Path string
        Err  error // the cause
}

func (e *PathError) Error() string
```

### 错误类型的问题

所以调用者可以使用类型断言或类型切换，错误类型必须公开。

如果你的代码实现了一个接口，该接口的定义需要一个特定的错误类型，那么该接口的所有实现者都需要依赖定义错误类型的包。

与调用者产生了很强的耦合，使得API变得很脆弱。

### 结论： 不用error types

虽然错误类型比哨兵错误值要好，因为它们可以捕获更多关于出错的上下文，但错误类型也有许多错误值的问题。

所以我的建议还是避免使用错误类型，或者至少避免将其作为公共API的一部分。



## Opaque errors

现在我们来看第三类错误处理。在我看来，这是最灵活的错误处理策略，因为它要求你的代码和调用者之间的**耦合度最小**。

我把这种风格称为不透明的错误处理，因为虽然你知道发生了一个错误，但你没有能力看到错误的内部。作为调用者，你只知道操作的结果是成功了，或者没有成功。

这就是不透明错误处理的全部内容--只需返回错误，而不假设其内容。如果你采用了这个方式，那么错误处理作为调试的辅助手段就会变得非常有用。

```go
import “github.com/quux/bar”

func fn() error {
        x, err := bar.Foo()
        if err != nil {
                return err
        }
        // use x
}
```

例如，Foo的定义并不保证它在错误的上下文中会返回什么。现在，Foo的作者可以自由地用额外的上下文来注释通过它的错误，而不破坏它与调用者的契约。

### 为行为断言错误， 而不是为类型

在少数情况下，这种二进制的错误处理方法是不够的。

例如，与进程外世界的交互，如网络活动，需要调用者调查错误的性质，以决定重试操作是否合理。

在这种情况下，我们不是断言错误是一个特定的类型或值，而是可以断言错误实现了一个特定的行为。来看看这个例子：

```go 
type temporary interface {
        Temporary() bool
}
 
// IsTemporary returns true if err is temporary.
func IsTemporary(err error) bool {
        te, ok := err.(temporary)
        return ok && te.Temporary()
}
```

我们可以将任何错误传递给isTemporary, 已确定是否可以重试错误

如果error没有实现temporary接口， 那么他就没有Temporary方法， 那这个error就不是临时的。

如果error实现了Temporary方法，那么如果Temporary返回真，调用者或许可以重试操作。

这里的关键是这个逻辑可以不需要导入定义错误的包，也不需要了解err的底层类型的情况下实现，我们只是对它的行为感兴趣。

### Don't just check errors, handle them gracefully

这就引出了我想说的第二句Go proverb。不要只是检查错误，要优雅地处理错误。你能不能指出下面这段代码的问题？

```go
func AuthenticateRequest(r *Request) error {
        err := authenticate(r.User)
        if err != nil {
                return err
        }
        return nil
}
```

一个显而易见的建议局势把这五行代码换成一行。

```go
return authenticate(r.User)
```

这种简单的事，大家都能在代码审查时发现。更根本的问题是，这段代码的问题是我**无法判断原来的错误来自哪里**。

如果authenticate返回一个错误，那么AuthenticateRequest会把错误返回给它的调用者，调用者可能也会这样做，以此类推。在程序的顶部，程序主体会将错误打印到屏幕或日志文件中，打印的内容是：No such file or directory。



Donovan和Kernighan的《Go编程语言》建议你使用fmt.Errorf为错误路径添加上下文。

```go
func AuthenticateRequest(r *Request) error {
        err := authenticate(r.User)
        if err != nil {
                return fmt.Errorf("authenticate failed: %v", err)
        }
        return nil
}
```

但如我们之间所见，将错误值转换成为字符串，与另一个字符串合并，然后用fmt.Errorf将其转换回错误，破坏了相等行也破坏了原始错误中的任何上下文。



### Annotating errors

我想推荐一种为错误添加上下文的方法。为此我将介绍一个简单的包。代码在github.com/pkg/errors。errors包有两个主要函数：

```go
// Wrap annotates cause with a message
func Wrap(cause error, message string) error
```

第一个是`Wrap`, 其接受一个错误，和一条信息然后产生一个新的错误。

```go
// Cause unwraps an annotated error
func Cause(err error) error
```

第二个函数是Cause，他接受一个可能被打包了的错误，拆包返回成原始的error。

用这两个函数，你现在能注解任何error，复原潜在的错误（recover the underlying error)，当我们需要检查它时。看下这个例子，一个函数将文件读入内存

```go
func ReadFile(path string) ([]byte, error) {
  f, err := os.Open(path)
  if err != nil {
    return nil, errors.Wrap(err, "open failed")
  }
  defer f.Close()
  
  buf, err := ioutil.ReadAll(f)
  if err != nil {
    return nil, errors.Wrap(err, "read failed")
  }
  return buf, nil
}
```

我们来用这个函数写一个读取config文件的函数，然后从main函数中调用它：

```go
func ReadConfig() ([]byte, error) {
  home := os.Getenv("HOME")
  config, err := ReadFile(filepath.Join(home, ".settings.xml"))
  return config, errors.Wrap(err, "could not read config")
}

func main() {
  _, err := ReadConfig()
  if err != nil {
    fmt.Println(err)
    os.Exit(1)
  }
}
```

如果ReadConfig的代码路径错了，因为我们用了errors.Wrap， 我们将得到一个K&D风格的干净整洁的有注释的错误。

```
could not read config: open failed: open /Users/dfc/.settings.xml: no such file or directory
```



因为errors.Wrap产生了一个错误栈，我们可以检查该栈已获得的额外的调试信息。还是这个例子，但这次我们用errors.Print来替换fmt.Println

```go
func main() {
  _, err := ReadConfig()
  if err != nil {
    errors.Print(err)
    os.Exit(1)
  }
}
```

我们将得到如下信息：

```
readfile.go:27: could not read config
readfile.go:14: open failed
open /Users/dfc/.settings.xml: no such file or directory
```

第一行来自ReadConfig，第二行来自ReadFile的os.Open部分，其余部分来自os包本身，他们本身不携带位置信息。

现在我们已经介绍了包装error已产生一个栈这个概念，我们需要反过来谈谈，如何解包他们。这是errors.Cause函数的领域。

```go
// IsTemporary returns true if err is temporary.
func IsTemporary(err error) bool {
        te, ok := errors.Cause(err).(temporary)
        return ok && te.Temporary()
}
```

每当你要检查错误是否与某一值或者类型匹配时，你应该首先将使用errors.Casuse函数将原始错误复原。

### 只处理错误一次

最后，我想提一下，你应该只处理一次错误。处理一个错误意味着检查错误值，并作出决定。

```go
func Write(w io.Writer, buf []byte) {
  w.Write(buf)
}
```

如果不用做决定，你大可以忽略这个error。如上所示，从w.Write产生的错误被丢弃了。

但当要做多余一个决定时，对于一个错误也是有问题的。

```go
func Write(w io.Writer, buf []byte) error {
        _, err := w.Write(buf)
        if err != nil {
                // annotated error goes to log file
                log.Println("unable to write:", err)
 
                // unannotated error returned to caller
                return err
        }
        return nil
}
```

在这个例子中，如果在Write的过程中发生了错误，就会有一行写到日志文件中，记下发生错误的文件和行数。错误也会返回给调用者，调用者可能会记录下来，然后返回，一直到程序的顶部。

所以你会在日志文件中得到一堆重复的行，但在程序的顶部你得到了没有任何上下文的原始错误。

有人知道Java吗？（？？）

```go
func Write(w io.Write, buf []byte) error {
  _, err := w.Write(buf)
  return errors.Wrap(err, "write failed")
}
```

通过使用errors包你可以给错误值添加上下文，这种方式既可以被人也可以被机器检查。



# 结论

总之，errors是你包的公共API的一部分。对待他们要像对待你的公共API的其他部分一样小心。

为了获得最大化的灵活性，我建议你尽量将所有的错误处理为不透明的。在你无法做到这一点的情况下，断言errors基于行为，而非类型或值。

尽量减少程序中sentinel error值的数量，通过对错误进行包装，将错误转换为不透明的错误。

最后，如果你需要去检查error，使用errors.Cause去复原