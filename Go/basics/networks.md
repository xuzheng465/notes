`netstat -an` 可以查看本机有哪些端口在监听

`netstat -anb` 来查看监听端口的pid， 在结合任务管理器关闭不安全的端口。



### tcp socket编程的快速入门



#### 服务端的处理流程

1. 监听端口 8888
2. 接收客户端的tcp链接，建立客户端和服务器端的链接
3. 创建goroutine， 处理该链接的请求（通常客户端会通过链接发送请求包。



#### 客户端的处理流程

1. 建立与服务器端的链接
2. 发送请求数据，接收服务器端返回的结果数据
3. 关闭链接



功能

客户端

能通过终端输入数据（输入一行发送一行），并发送给服务器端

在终端输入exit表示退出程序。



## 海量用户即时通讯系统



### 需求分析

1. 用户注册
2. 用户登录
3. 显示在线用户列表
4. 群聊（广播）
5. 点对点聊天
6. 离线留言



#### 在golang中使用redis



### Redis

NoSQL 数据库

https://redis.io

REmote Dictionary Server (远程字典服务器)， Redis性能非常高，单机能够达到15w qps

通常适合做缓存，也可以持久化。

高性能的（key/value）分布式内存数据库。基于内存运行并支持持久化的NoSQL数据库



内存数据

string

hash

list

set

zset 有序集合



默认零号数据库

#### Redis 常用命令

##### 切换数据库

select database 1

select 1

##### 查看当前数据库的key-val数量

dbsize

##### 清空当前数据库的key-val

flushdb

##### 清空所有数据库的key-val

flushall



string 



```
SETEX key seconds value
```

将值value关联到key，并将key的生存时间设为seconds（以秒为单位）

如果key已经存在，SETEX命令将覆写旧值。

这个命令等价于：

```
SET key value
EXPIRE key seconds
```



##### MSET 

MSET key value [key value ... ]



#### hash

Redis hash是一个stirng类型的field和value的映射表，hash特别适合用于存储对象。

```
user1 name "smith" age 30
```

key: user1

name: 张三 和 age 30 就是两对 field-value

##### hget



##### hgetall



##### hdel



##### hmset

```
hmset user2 name jerry age 110 job "java coder"
```



##### hmget

```
hmget user2 name age job
```



##### hlen



##### hexists

```
hexists user2 name
```



#### list

列表

按照插入顺序排序

本质是链表，list元素是有序的，可重复



##### lpush

左边插入

```
lpush city beijing shanghai tianjing
```

##### lrange

LRANGE key start stop

```
lrange city 0 -1 
```

0代表从0位置开始取

-1表示全部取完

#### Set

##### sadd

```
sadd emails tom@gamil.com jack@hotmail.com
```



##### smembers





