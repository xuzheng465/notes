#### 统计string s 中 substr 的非重叠实例的数量

```go
func Count(s, substr string) int

strings.Count("cheese", "e") //3
strings.Count("five", "") // before and after each rune : 5
```



#### Go 比较两个字符串

```go
strings.Compare("ab", "ba")
```

#### # %v in fmt

```go
the value in a default format
```

#### func MultiWriter(writers ...Writer) Writer

```go
封装多个Writer， 
统一的Writer操作可以将数据同时写入多目标
获得一个可以对多个Writer同时写入的writer
```

#### func Pipe() (*PipeReader, *PipeWriter)

```go
返回值： PipeReader管道读取器， PipeWriter管道写入器
reader, writer := io.Pipe()
```

#### Mutex是什么的缩写

```go
A mutual exclusion lock
```

#### io.Copy() 的签名及功能

```go
func Copy(dst Writer, src Reader) (written int64, err error)
// 向dst 拷贝src的全部数据； 读取src中数据直到EOF， 故不会返回io.EOF
```

#### Convert string to bytes in Go

```go
b := []byte("ABC")
```

#### Implement a `Reader` type that emits an infinite stream of the ASCII character `'A'`.

```go
type MyReader struct{}
func (mr *Myreader) Read([]byte) (int, error) {
  for i:=range b {
    b[i] = 'A'
  }
  return len(b), nil
}
```

#### 在Go中，将十进制数字转化成字符串

```go
strconv.Itoa(time.Now().Year())
```

#### func (b *Reader) ReadString(delim byte) (line string, err error)

```go
参数： delim byte 分隔字节
返回值：
	line 字符串
	err	错误
ReadString读取数据直到delim第一次出现，返回一个包含delim的字符串。
如果ReadString在读取delim前遇到错误，它返回已读字符串和那个错误（通常是io.EOF）
只有当返回的字符串不以delim结尾 时，ReadString 才返回非空err
```

#### Convert a slice of bytes in lowercase in Golang	

```go
res := bytes.ToLower([]byte("TESTING"))
```

#### 创建一个strings.Reader并以每次8字节的速度读取它的输出

```go
func main() {
  r := strings.NewReader("Hello, Reader!")
  b := make([]byte, 8)
  
  for {
    n, err := r.Read(b)
    fmt.Printf("n=%v err=%v b=%v\n", n, err, b)
    fmt.Printf("b[:n] = %q\n", b[:n])
    if err == io.EOF {
      break
    }
  }
}
```

#### func NewReaderSize(rd io.Reader, size int) *Reader

```go
rd io.Reader 接口， 创建的Reader对象会从此接口读取数据
size 指定缓存的大小（字节数）
创建支持缓存读取的，具有指定长度缓冲区的Reader对象，Reader对象会从底层的io.Reader接口读取尽量多的数据进行缓存。
```

#### Go替换字符串

```go
func Replace(s, old, new string, n int) string

fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2))
// oinky oinky oink
fmt.Println(strings.Replace("oink oink oink" ,"moo", -1))
// moo moo moo
```

#### io.Reader 接口有一个Read方法

```go
func (T) Read(b []byte) (n int, err error)
// Read 用数据填充给定的字节切片并返回填充的字节数和错误值
```

#### 

```go
// byte type, one of the Go's basic type, used for holding raw data, such as you might read from a file or network connection
```

#### Print the commands but do not run them in go

```go
go build -n
```

#### func Fprintf(w io.Writer, format string, a ...interface{}) (int, error)

```go
w 写入器
format 打印的格式说明
a ... 值变量列表
功能：这个函数用来根据说明格式字符串和参数列表生成一个打印字符串。
```

#### 

```go

```

#### 

```go

```

#### 