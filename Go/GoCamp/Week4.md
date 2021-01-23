# 第四课 Go 工程化实践



## 工程项目结构



[Standard Go Project Layout](https://github.com/golang-standards/project-layout/blob/master/README_zh.md)



`/cmd`

本项目的主干

每个应用程序的目录名都应该与你想要的可执行文件的名称相匹配（例如，/cmd/myapp）

不要在这个目录放置太多代码。如果你认为代码可以导入并在其他项目中使用，那么它应该位于/pkg目录中。

如果代码不是可重用的，或者你不希望其他人重用它，请将该代码放到/internal目录中

<img src="/Users/xuzheng/Projects/notes/Go/GoCamp/Week4.assets/go-project-cmd.png" alt="go-project-cmd" style="width:300px; " />





`/internal`

私有应用程序和库代码。这是你不希望其他人在其应用程序或库中导入的代码。请注意这个布局模式是编译器本身执行的。

注意，你并不局限于顶级 internal 目录。在项目树的任何级别上都可以有多个内部目录。
你可以选择向 internal 包中添加一些额外的结构，以分隔共享和非共享的内部代码。这不是必需的(特别是对于较小的项目)，但是最好有有可视化的线索来显示预期的包的用途。你的实际应用程序代码可以放在 `/internal/app` 目录下(例如 `/internal/app/myapp`)，这些应用程序共享的代码可以放在 `/internal/pkg` 目录下(例如 `/internal/pkg/myprivlib`)。



`/pkg`

外部应用程序可以使用的库代码（例如`/pkg/mupubliclib`)。其他项目导入这些库

/pkg目录仍然是一种很好的方法，可以显式地表示该目录中的代码对于其他人来说是安全使用的好方法。

/pkg目录内，可以参考go标准库的组织方式，按照功能分类



当个目录包含大量非Go组件和目录时，这也是一种将Go代码分组到一个位置的方法，这使得运行各种Go工具变得更加容易组织。



<img src="/Users/xuzheng/Projects/notes/Go/GoCamp/Week4.assets/go-project-pkg.png" alt="go-project-pkg" style="width:300px; " />



<img src="/Users/xuzheng/Projects/notes/Go/GoCamp/Week4.assets/go-project-root-1.png" alt="go-project-root-1" style="width:300px;" />



### Kit Project Layout

每个公司都应当为不同的微服务建立一个统一的 kit 工具包项目(基础库/框架) 和 app 项目。
基础库 kit 为独立项目，公司级建议只有一个，按照功能目录来拆分会带来不少的管理工作，因此建议合并整合。



kit项目必须具备的特点：

* 统一
* 标准库方式布局
* 高度抽象
* 支持插件

<img src="/Users/xuzheng/Projects/notes/Go/GoCamp/Week4.assets/go-project-kit.png" alt="go-project-kit" style="width:300px;" />



### Service Application Project layout

* `/api`

API 协议定义目录，xxapi.proto protobuf 文件，以及生成的 go 文件。我们通常把 api 文档直接在 proto 文件中描述。

* `/configs`

配置文件模板或默认配置。

* `/test`

额外的外部测试应用程序和测试数据。你可以随时根据需求构造` /test` 目录。对于较大的项目，有一个数据子目录是有意义的。例如，你可以使用 /test/data 或 /test/testdata (如果你需要忽略目录中的内容)。请注意，Go 还会忽略以“.”或“_”开头的目录或文件，因此在如何命名测试数据目录方面有更大的灵活性。

不应该包含 `/src`



一个gitlab的project里可以放置多个微服务的app（类似monorepo）。也可以按照gitlab的group里创建多个project，每个project对应一个app

* 多 app 的方式，app 目录内的每个微服务按照自己的全局唯一名称，比如 “`account.service.vip`” 来建立目录，如: `account/vip/*`。
* 和 app 平级的目录 pkg 存放业务有关的公共库（非基础框架库）。如果应用不希望导出这些目录，可以放置到 `myapp/internal/pkg` 中。





微服务中的app服务类型分为4类：interface、service、job、admin

* interface： 对外的BFF服务，接收来自用户的请求，比如暴露了HTTP/gRPC接口
* service： 对内的微服务，仅接受来自内部其他服务或者网关的请求，比如暴露了gRPC接口只对内服务
* admin：区别于service，更多是面向运营侧的服务，通常数据权限更高，隔离带来更好的代码级别安全
* job：流式任务处理的服务，上游一般依赖message broker
* task：定时任务，类似cronjob，部署到task托管平台中



cmd应用目录负责程序的：启动，关闭，配置初始化等。









## API设计











## 配置管理









## 包管理

go mod



https://github.com/gomods/athens
https://goproxy.cn

https://blog.golang.org/modules2019
https://blog.golang.org/using-go-modules
https://blog.golang.org/migrating-to-go-modules
https://blog.golang.org/module-mirror-launch
https://blog.golang.org/publishing-go-modules
https://blog.golang.org/v2-go-modules
https://blog.golang.org/module-compatibility





## 测试



* 小型测试带来优秀的代码质量、良好的异常处理、优雅的错误报告；大中型测试会带来整体产品质量和数据验证。
* 不同类型的项目，对测试的需求不同，总体上有一个经验法则，即70/20/10原则：70%是小型测试，20%是中型测试，10%是大型测试。
* 如果一个项目是**面向用户**的，拥有较高的集成度，或者用户接口比较复杂，他们就应该有更多的中型和大型测试；如果是基础平台或者面向数据的项目，例如索引或网络爬虫，则最好有大量的小型测试，中型测试和大型测试的数量要求会少很多。





## References







