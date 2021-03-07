功能

* 在不访问服务器的前提下满足客户端的HTTP请求



* 缩短客户请求的响应时间

* 减少机构的流量

* 在大范围（Internet）实现有效的内容分发





![hnet-web-cache-1](/Users/xuzheng/Projects/notes/Networking/Hagongda/2-web cache.assets/hnet-web-cache-1.png)



用户设定浏览器通过缓存进行Web访问

* 浏览器向缓存/代理服务器发送所有的HTTP请求
  * 如果所请求对象在缓存中，缓存返回对象
  * 否则，缓存服务器向原始服务器发送HTTP请求，获取对象，然后返回客户端并保存该对象
* 缓存既充当client 也server
* 一般由ISP（Internet服务提供商）架设







### 条件性GET方法

目标：

​	如果缓存有最新的版本，则不需要发送请求对象

缓存：

* 在HTTP请求消息中声明所持有版本的日期
* `if-modified-since: <date>`

服务器：

* 如果缓存的版本是最新的，则响应消息中不包含对象
* `HTTP/1.0 304 Not Modified`

