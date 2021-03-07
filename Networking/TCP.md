seq是序列号，这是为了连接以后传送数据用的，ack是对收到的数据包的确认，值是等待接收的数据包的序列号。 在第一次消息发送中，A随机选取一个序列号作为自己的初始序号发送给B；第二次消息B使用ack对A的数据包进行确认，因为已经收到了序列号为x的数据包，准备接收序列号为x+1的包，所以ack=x+1，同时B告诉A自己的初始序列号，就是seq=y；第三条消息A告诉B收到了B的确认消息并准备建立连接，A自己此条消息的序列号是x+1，所以seq=x+1，而ack=y+1是表示A正准备接收B序列号为y+1的数据包。 seq是数据包本身的序列号；ack是期望对方继续发送的那个数据包的序



### TCP连接传输三个阶段：

1. 连接建立
2. 数据传送
3. 连接释放



TCP连接采用**客户服务器方式**， 主动发起连接建立的应用进程叫**客户**，而被动等待连接建立的应用进程叫**服务器**。



两个情况下SYN=1（同步位z：

1. 连接请求报文段
2. 连接请求的确认报文段