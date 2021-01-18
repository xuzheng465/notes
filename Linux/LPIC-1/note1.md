

## 安装





显示显卡信息

```

$ ip a s

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:f3:4e:91 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.4/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 426sec preferred_lft 426sec
    inet6 fe80::7e65:f6e1:a045:97a/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:8f:aa:6a brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.101/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 424sec preferred_lft 424sec
    inet6 fe80::83a1:c21:43bb:2c93/64 scope link noprefixroute
       valid_lft forever preferred_lft forever

```



Network Manager Command-Line Tools

* nmtui: Provides a simple text-based menu tool

* nmcli: Provides a text-only command-line tool



```
$ nmcli conn show
最开始没有enp0s8
$ nmcli conn up enp0s8

NAME    UUID                                  TYPE      DEVICE
enp0s3  719449cd-adfc-4219-8b65-006404447ffd  ethernet  enp0s3
enp0s8  c9a18ac7-f67f-4119-b254-916f26eec50d  ethernet  enp0s8

```



```

sed -i s/ONBOOT=no/ONBOOT=yes/ /etc/sysconfig/network-scripts/ifcfg-enp0s3

```



```

grep ONBOOT !$

```



```

yum install -y redhat-lsb-core net-tools epel-release kernel-headers kernel-devel 

```



```

yum groupinstall -y "Development Tools"
```



```

yum groupinstall -y "X Window System" "MATE Desktop"

```



```

systemctl set-default graphical.target

systemctl isolate graphical.target

```



```



```



```


```



```


```



```


```

