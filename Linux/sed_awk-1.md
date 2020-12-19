Intro

sudo systemctl is-active httpd

sudo systemctl status httpd

sudo systemctl start httpd

grep



```bash
grep user /etc/passwd
```





```bash
declare -f | grep '^[a-z_]'
```



删除# 行 和空格行

```bash
sed -i.commented '/^#/d;/^$/d' /etc/ntp.conf
```



create a backup

```bash
sed -i.bak ' /^#/ d ; /^$/ d' /etc/ntp.conf
```





```bash
sed -n 'p' /etc/passwd
```





```bash
sed -n ' /^user/ p' /etc/passwd
```



## awk

```bash
awk -W version
```



isolates the MAC address from a network interface

```bash
ifconfig eth0 | awk -F":"'/HWaddr/{printt $3 $4 $5 $6 $7}'
```





```bash
awk '{ print } ' /etc/ntp.conf
```



# grep

Global Regular Expression and Print

a text search utility used from the Linux command line to globally search file or STDIN for a given regular expression.

It will then print matching lines to STDOUT.

* Search config files for setting
* Hardware is mainly undocumented so D wants to document the CPU cores in each server
* Process catalog data stored in CSV files



- Display transmission data from ifconfig

```bash
ifconfig enp0s5 | grep RX
```

* Show PAM configurations that include a specific module

```bash
grep pam_nologin /etc/pam.d/*
```

* Count the number of CPU cores in a host

```bash
grep -c name /proc/cpuinfo
```



```bash
./parsecsv.sh tools | grep -A2 hammer
```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```

