## 服务端启动流程



```java
public class NettyServer {
    public static void main(String[] args) {
        NioEventLoopGroup bossGroup = new NioEventLoopGroup();
        NioEventLoopGroup workerGroup = new NioEventLoopGroup();

        ServerBootstrap serverBootstrap = new ServerBootstrap();
        serverBootstrap
                .group(bossGroup, workerGroup)
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<NioSocketChannel>() {
                    protected void initChannel(NioSocketChannel ch) {
                    }
                });

        serverBootstrap.bind(8000);
    }
}
```

* 我们创建了两个`NioEventLoopGroup`，这两个对象可以看作是传统IO编程模型的两大线程组，
  * `bossGroup`表示监听端口，`accept`新连接的线程组，
  * `workGroup`表示每一条连接的数据读写的线程组。
  * 用生活中的例子来讲就是，一个工厂要运作，必然要有一个老板负责从外面接活，然后有很多员工，负责具体干活，老板就是`bossGroup`，员工就是`workerGroup`，`bossGroup`接收完连接，扔给`workerGroup`去处理。
  * 接下来，我们创建了一个引导类`ServerBootstrap`， 这个类将引导我们进行服务端的启动工作，直接new出来开搞
  * 通过`.group(bossGroup, workerGroup)` 给引导类配置两大线程组，这个引导类的==线程模型==也就定型了
  * 指定我们服务端的IO模型为NIO，我们通过`.channel(NioServerSocketChannel.class)` 来指定IO模型
    * 如果你想指定IO模型为BIO，那么这里配置上`OioServerSocketChannel.class`类型即可，一般不会这么做，Netty的优势在于NIO
  * 接着我们调用`childHandler()`方法，给这个引导类创建一个`ChannelInitializer`， 这里就是定义后续每条连接的数据读写，业务处理逻辑。
    * 在ChannelInitializer这个类中，我们注意到有一个泛型参数`NioSocketChannel`，这个类呢，就是netty对NIO类型的连接的抽象，
    * 而我们前面`NioServerSocketChannel`也是对NIO类型的连接的抽象，`NioServerSocketChannel`和NioSocketChannel的概念可以和BIO编程模型中的`ServerSocket`以及Socket两个概念对应上



要启动一个Netty的服务端，必须制定三类属性：

1. 线程模型
2. IO模型
3. 连接读写处理逻辑



### 自动绑定递增端口



`serverBootstrap.bind(8000)` 是一个异步的方法，调用之后是立即返回的，他的返回值是一个`ChannelFuture`， 我们可以给这个ChannelFuture添加一个监听器GenericFutureListener, 然后我们在GenericFutureListener的operationComplete方法里面，我们可以监听端口是否绑定成功，接下来是监测端口是否绑定成功的代码片段。

```java
serverBootstrap.bind(8000).addListener(new GenericFutureListener<Future<? super Void>>() {
    public void operationComplete(Future<? super Void> future) {
        if (future.isSuccess()) {
            System.out.println("端口绑定成功!");
        } else {
            System.err.println("端口绑定失败!");
        }
    }
});

```

我们接下来从1000端口号，开始往上找端口号，直到端口绑定成功，我们要做的就是在`if(future.isSuccess)`的else的逻辑里面重新绑定一个递增的端口号。

```java
private static void bind(final ServerBootstrap serverBootstrap, final int port) {
    serverBootstrap.bind(port).addListener(new GenericFutureListener<Future<? super Void>>() {
        public void operationComplete(Future<? super Void> future) {
            if (future.isSuccess()) {
                System.out.println("端口[" + port + "]绑定成功!");
            } else {
                System.err.println("端口[" + port + "]绑定失败!");
                bind(serverBootstrap, port + 1);
            }
        }
    });
}
```

以上代码中的最关键的就是在端口绑定失败之后，重新调用自身方法，并且把端口号加一，

```java
bind(serverBootstrap, 1000);
```

## 服务端启动其他方法

### handler（）方法

```java
serverBootstrap.handler(new ChannelInitilizer<NioServerSocketChannel>() {
  protected void initChannel (NioServerSocketChannel ch) {
    System.out.println("服务端启动中");
  }
}
```

`childHandler()` 用于指定处理新连接数据的读写处理逻辑，`handler()` 用于指定在服务端启动过程中的一些逻辑。

通常不用

### attr() 方法

```java
serverBootstrap.attr(AttributeKey.newInstance("serverName"), "nettyServer");
```

attr（）方法可以给服务端的channel，也就是NioServerSocketChannel指定一些自定义属性，然后我们可以通过channel.attr（）取出这个属性

指定我们服务端channel的一个serverName属性，属性值为nettyServer，其实说白了就是给NioServerSocketChannel维护一个map而已

### childAttr() 方法

```java
serverBootstrap.childAttr(AttributeKey.newInstance("clientKey"), "clientValue");
```

给每一条连接指定自定义属性，可通过`channel.attr()`取出该属性



### childOption() 方法

```java
serverBootstrap
  .childOption(ChannelOption.SO_KEEPALIVE, true)
  .childOption(ChannelOption.TCP_NODELAY, true)
```

这个方法给每条连接设置一些TCP底层相关的属性

* ChannelOption.SO_KEEPALIVE 表示是否开启TCP底层心跳机制，true为开启
* ChannelOption.TCP_NODELAY 表示是否开启Nagle算法，true表示关闭，false表示开启，
  * 如果要求高实时性，有数据发送时就马上发送，就关闭这个选项。
  * 如果需要减少发送次数减少网络交互，就开启



### option() 方法

除了给每个连接设置这一系列属性之外，我们还可以给服务端channel设置一些属性，最常见的就是so_backlog, 如下设置

```java
serverBootstrap.option(ChannelOption.SO_BACKLOG, 1024);
```

表示系统用于临时存放已完成三次握手的请求的队列的最大长度，如果连接建立频繁，服务器处理创建新连接较慢，可以适当调大这个参数



## 总结

Netty服务端启动的流程：创建一个引导类，然后给他指定**线程模型**，IO模型，连接读写处理逻辑，绑定端口之后



