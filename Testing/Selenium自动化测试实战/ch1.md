# 第一章 Selenium起步



## Web 自动化测试

### 1. 什么是测试

* 通俗：程序测试就是运行程序，并发现程序的错误

* 专业：验证软件的正确性、完整性、安全性和质量的过程
* 用程序猿的话说就是找bug

### 2. 测试的分类

* #### 按开发阶段分

  * 单元测试
    * 模块测试
    * 对软件组成的单位进行测试
    * 目的：检测软件的组成单位的正确性
  * 集成测试
    * 组装测试
    * 程序的模块适当的组成策略，组装在一起对程序的接口，集成后的功能进行正确性的一个检测工作。
  * 系统测试
    * 将软件系统看成一个整体来测试，给他的功能、性能、软件所在的运行的软硬件环境进行测试
  * 验收测试

* #### 按是否查看代码划分

  * 黑盒测试
    * 不查看代码
    * 把待测软件当作一个黑盒子，不关心其内部结构，
    * 只关心它的输入数据和输出数据
    * 
  * 白盒测试
    * 研究源代码
  * 灰盒测试
    * 既有功能性测试又有代码测试

* 按是否运行划分·

  * 静态测试
    * 不运行，通过分析源码的方式，比如说语法结构，过程接口，检查正确性
    * 软件设计说明书
    * 程序流程图
  * 动态测试
    * 运行起来
    * 检查运行结果和预期结果差异与否
    * 检查健壮性

* 按测试对象划分

  * 性能测试
  * 安全测试
  * 兼容性测试
    * 软件之间相互协作能否良好运行
    * 是否影响其他软件硬件性能发挥
  * 文档测试:question:
    * 开发文档、用户文档、管理文档是否完整
  * 用户体验测试
    * 易用性、交互性
  * 业务测试
    * 模拟真实用户流程
  * 界面测试
  * 安装测试
  * 内存泄漏测试

* 按测试实施的组织

  * 阿尔法测试
    * 初期测试
  * 贝塔测试
    * 验收测试
    * 交给最终用户，看是否在不同场合下面是否可以运行
  * 第三方测试

* 按是否手工执行划分

  * 手工测试
  * 自动化测试
    * 机器参与
    * 预先有一些数据，把数据准备好之后，通过程序的方式自动地读取数据，验证最终的结果。

* 其他分类

  * 冒烟测试
    * 针对不同版本，每次需求更改之后，在正式测试前，对系统进行简单的测试。
    * 自测
  * 回归测试
    * 修改代码之后，看看是否引起其他的问题。

### 3. 什么是自动化测试

机器来运行，可以是单机，可以是阵列



### 4. 自动化测试的技术选型

本课：Python+Selenium



## Selenium三剑客

WebDriver, IDE, Grid

Selenium 是一个用于Web应用程序自动化测试工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。

* 主要功能包括：
  * 测试与浏览器的兼容性，测试你的应用程序看是否能够很好的工作在不同浏览器和操作系统之上
  * 测试系统功能，创建回归测试检验软件功能和用户需求。
    * 回归测试



### WebDriver

Selenium WebDriver是客户端API接口，测试人员通过调用这些接口，来访问浏览器驱动，浏览器驱动再访问浏览器。

<img src="/Users/xuzheng/Projects/notes/Testing/Selenium自动化测试实战/ch1.assets/image-20210106175611671.png" alt="image-20210106175611671" style="width:700px;" />

### Selenium IDE

浏览器插件，可以将手动测试过程记录下来，并生成自动化测试脚本，可以实现回放

对初学者，学习脚本和回归测试都很有帮助。



### Selenium Grid

通过在多台计算机上进行分布式来扩容，并从一个中心点管理多个环境，从而轻松地对多种浏览器/OS组合运行测试，那么可以使用Selenium Grid



### 特点

多浏览器支持

多语言支持

支持分布式测试（使用Selenium Grid）

支持录制、回放和脚本生成

