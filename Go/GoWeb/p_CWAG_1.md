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

