# NAT



网络地址转换（Network Address Translation, **NAT** 网络掩蔽、IP掩蔽)

在计算机网络中是一种在IP数据包通过路由器或防火墙时重写来源IP地址或目的IP地址的技术。



这种技术被普遍使用在有多台主机但只通过一个公有IP地址访问[互联网](https://zh.wikipedia.org/wiki/網際網路)的[私有网络](https://zh.wikipedia.org/wiki/私有网络)中



### NAT network

在virtualbox中，**NAT network**

Guest can access internet, systems on host's network and each other



vm间可以交流

<img src="/Users/xuzheng/Projects/notes/MIT/websecurity/NAT.assets/nat-network.png" alt="nat-network" style="zoom:67%;" />

### Bridged adapter

* Guests share the host's network adapter

If we need the guests to participate directly as machines on the host's network we can enable **bridge mode** which joins the guests to the same network as the hosts by sharing the host's network interface



<img src="/Users/xuzheng/Projects/notes/MIT/websecurity/NAT.assets/bridged-adapter.png" alt="bridged-adapter" style="width:700px;" />

### Internal network

<img src="/Users/xuzheng/Projects/notes/MIT/websecurity/NAT.assets/internal-network.png" alt="internal-network" style="width:700px;" />





## DHCP

动态主机设置协议 （Dynamic Host Configuration Protocol)

