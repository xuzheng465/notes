文件就是数据源(保存数据的地方)

流: 数据在数据源(文件)和程序(内存)之间经历的路径

输入流: 数据从数据源(文件)到程序的路径 (读文件)

输出流: 数据从程序(内存) 到数据源文件的路径 (写文件)



os包里边有个结构体 **File**

```go
type File struct {
  
}
```

`File` represents an open ***file descriptor*** 

文件对象,文件指针,文件句柄, 文件描述符



func **Create**

```go
func Create(name string) (file *File, err error)
```

Create采用模式0666（任何人都可读写，不可执行）创建一个名为name的文件，如果文件已存在会截断它（为空文件）。如果成功，返回的文件对象可用于I/O；对应的文件描述符具有O_RDWR模式。如果出错，错误底层类型是*PathError。



func **Open**

```go
func Open(name string) (file *File, err error)
```

Open打开一个文件用于读取。如果操作成功，返回的文件对象的方法可用于读取数据；对应的文件描述符具有O_RDONLY模式。如果出错，错误底层类型是*PathError。

func **OpenFile**

```go
func OpenFile(name string, flag int, perm FileMode) (file *File, err error)
```

OpenFile是一个更一般性的文件打开函数，大多数调用者都应用Open或Create代替本函数。它会使用指定的选项（如O_RDONLY等）、指定的模式（如0666等）打开指定名称的文件。如果操作成功，返回的文件对象可用于I/O。如果出错，错误底层类型是*PathError。

Example 1:

```go
package main 
import (
	"log"
  "os"
)

func main() {
  f, err := os.OpenFile("notes.txt", os.O_RDWR|os.O_CREATE, 0755)
  if err != nil {
    log.Fatal(err)
  }
  if err := f.Close(); err!=nil {
    log.Fatal(err)
  }
}
```

Example 2

```go
func main() {
  // If the file doesn't exist, create it, or append to the file
  f, err := os.OpenFile("access.log", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
  if err != nil {
    log.Fatal(err)
  }
  if _, err := f.Write([]byte("appended some data\n")); err != nil {
    f.Close()
    log.Fatal(err)
  }
  if err:=f.Close(); err != nil {
    log.Fatal(err)
  }
}
```



### bufio

创建一个*Reader, 带缓冲

Package bufio implements buffered I/O. It wraps an io.Reader or io.Writer object, creating another object (Reader or Writer) that also implements the interface but provides buffering and some help for textual I/O.

bufio包实现了**有缓冲的I/O**。它包装一个io.Reader或io.Writer接口对象，创建另一个也实现了该接口，且同时还提供了缓冲和一些文本I/O的帮助函数的对象。

`defaultBufSize = 4096` 

```go
reader:= bufio.NewReader(file)
for {
  str, err := reader.ReadString('\n') // 读取到一个换行就结束
  if err == io.EOF {
    fmt.Print(str)
    break
  }
  if err != nil {
    log.Fatal(err)
  }
  fmt.Print(str)
}
fmt.Println("Finish reading file...")
```

```go
func (b *Reader) ReadString(delim byte) (line string, err error)
```

_**ReadString**_读取直到第一次遇到delim字节，返回一个包含已读取的数据和delim字节的字符串。如果ReadString方法在读取到delim之前遇到了错误，它会返回在错误之前读取的数据以及该错误（一般是io.EOF）。当且仅当ReadString方法返回的切片不以delim结尾时，会返回一个非nil的错误。

### 拷贝文件

```go
func Copy(dst Writer, src Reader) (written int64, err error)
```



### example

```go
func CopyFile(dstFileName string, srcFileName string) (written int64, err error) {
  srcFile, err:=os.Open(srcFilename)
  if err!=nil {
    log.Fatal(err)
  }
  defer srcFile.Close()
  // 通过srfile, 获取到Reader
  reader := bufio.NewReader(srcFile)
  
  // 打开dstFileName
  file, err := os.OpenFile(dstFileName, os.O_WRONLY | os.O_CREATE, 0666)
  if err != nil {
    log.Fatal(err)
  }
  // 通过dstFile, 获取到Writer
  writer := bufio.NewWriter(dstFile)
  defer dstFile.Close()
  
  return io.Copy(writer, reader)
}
```

