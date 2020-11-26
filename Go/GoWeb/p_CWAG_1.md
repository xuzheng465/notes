Project code:

`~/Project/gocode`

## MVC

### Controller

* 接受HTTP请求
* 将逻辑委托给模型(model)
* 准备返回数据给视图(view)
* 处理路由(routing)
* Integration point for middleware
  *  but middleware kind of has controller and model components. So the middleware will register itself with the controller layer, but then it's going to apply some sort of business logic to that request that's typically a model layer concern. 

## Model

* Contains business logic and data
* Interacts with the backend services
  * databases
  * web services



## View

* Initiates requests to controller layer
* Uses controller response to update visuals



## Built in Handlers

* NotFoundHandler

* RedirectHandler

  * `func RedirectHandler(url string, code int) Handler`

* StripPrefix

  * ```go
    func StripPrefix(prefix string, h handler) Handler
    ```

  * The first argument that it's going to take is a prefix that is going to remove from the incoming request URL. 

  * The second argument is a handler that's going to receive the request after the StripPrefix is done. 相当于装饰器, 裁去prefix, 转到留下来的url handler中

* TImeoutHandler

  * ```go
    func TimeoutHandler(h handler, dt time.Duration, msg string) Handler
    ```

  * 第一个参数: 被装饰得handler

  * 第二个参数: 允许第一个参数运行的时间

  * 如果超出时间, 第三个参数返回给调用者, 让他知道等待返回时间过长.

* FileServer

  * ```go
    func FileServer(root FileSystem) Handler
    ```

  * ```go
    type FileSystem interface {
      Open(name string) (File, error)
    }
    type Dir string
    func (d Dir) Open(name string) (File, error)
    ```

  * 

## Templates

### Must

Must is a helper that wraps a call to a function returning (*Template, error) and panics if the error is non-nil.  It is intended for use in variable initializations such as

```go
var t = template.Must(template.New("name").Parse("html"))
```





### ParseFiles

```go
func (t *Template) ParseFiles(filenames ...string) (*Template, error)
```

ParseFiles parses the named files and associates the resulting templates with t. If an error occurs, parsing stops and the returned template is nil; otherwise it is t. There must be at least one file. 





### ParseGlob

ParseGlob parses the template definitions in the files identified by the pattern and associates the resulting templates with t. The files are matched according to the semantics of filepath.Match, and the pattern must match at least one file. ParseGlob is equivalent to calling t.ParseFiles with the list of files matched by the pattern.

ParseGlob 解析***模式字符***串所标识的文件中的模板定义，并将结果中的模板与 t 相关联。文件根据 filepath.Match 的语义进行匹配，并且模式必须至少匹配一个文件。ParseGlob相当于用模式匹配的文件列表调用t.ParseFiles。

编者注: 相当于正则表达式版ParseFiles