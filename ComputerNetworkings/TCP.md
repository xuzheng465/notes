# TCP



Packet Delay = (Data Size / Link Bandwidth) + Link Latency



## Basic rules of TCP



* ByteStream in each direction
* Every byte tagged with its place in sequence. Also:
  * Beginning of stream counts as one place in sequence
  * End of stream also counts as one
* 接收方告诉发送方:
  * 下一个所需字节
  * 还需要接受多少字节



### Format of each TCP segment

* 发送方to接收方
  * 