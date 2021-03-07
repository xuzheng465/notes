## SMTP协议： RFC 2821

* 使用TCP进行email消息的可靠传输
* 端口25
* 传输过程的三个阶段：
  * 握手
  * 消息的传输
  * 关闭
* 命令/响应交互模式
  * 命令（command）：ASCII文本
  * 响应（response）：状态代码和语句
* Email消息只能包含7位ASCII码

使用持久性连接

要求消息必须由7位ASCII码构成

SMTP服务器利用CRLF.CRLF确定消息的结束



* 与HTTP对比：
  * HTTP：拉式（pull）
  * SMTP：推式（push）
  * 都使用命令/响应交互模式
  * 命令和状态码都是ASCII码
  * HTTP：每个对象封装在独立的响应消息中
  * SMTP：多个对象在由多个部分构成的消息中发送
* 