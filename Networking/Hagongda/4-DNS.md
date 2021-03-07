* Internet上主机/路由器的识别问题
  * IP地址
  * 域名：www.hit.edu.cn
* 问题：域名和IP地址之间如何映射？
* 域名解析系统DNS
  * 多层命名服务器构成的分布式数据库
  * 应用层协议：完成名字的解析
    * Internet核心功能，用应用层协议实现
    * 网络边界复杂



## DNS服务

* 域名向IP地址的翻译
* 主机别名
* 邮件服务器别名
* 负载均衡：Web服务器

问题：为什么不使用集中式的DNS？

* 单点失败问题
* 流量问题
* 距离问题
* 维护性问题



## 迭代查询

* 被查询服务器返回域名解析服务器的名字
* “我不认识这个域名，但是你可以问这个服务器”

<img src="/Users/xuzheng/Projects/notes/Networking/Hagongda/4-DNS.assets/hnet-dns-迭代查询.png" alt="hnet-dns-迭代查询" style="zoom:50%;" />

## 递归查询

![hnet-dns-递归查询](/Users/xuzheng/Projects/notes/Networking/Hagongda/4-DNS.assets/hnet-dns-递归查询.png)

