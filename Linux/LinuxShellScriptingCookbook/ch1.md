



`%-5s`

一个格式为左对齐且宽度为5的字符串替换（`-`表示左对齐）

echo 会在输出文本的尾部追加一个换行符。

可以用`-n`来禁止这种行为。



#### 打印彩色输出

文本颜色是由对应的色彩码来描述的。其中包括：重置=0，黑色=30，红色=31，绿色=32，

黄色=33，蓝色=34，洋红=35，青色=36，白色=37。

```bash
echo -e "\e[1;31m This is red text \e[0m"
```



```bash
echo -e "\e[1;42m Green Background \e[0m"
```



**tr**

translate or delete characters

```bash
$ cat /proc/$PID/environ | tr '\0' '\n'
```

#### export

export命令声明了将由子进程所继承的一个或多个变量。

这些变量被导出后，当前shell脚 本所执行的任何应用程序都会获得这个变量。

#### 获得字符串的长度

```bash
length=${#var}

var=12345678901234567890
echo ${#var}
```

#### check current shell

```bash
if [ $UID -ne 0 ] ; then
	echo Non root user. Please run as root.
else
	echo Root user
fi
```



```bash
if test $UID -ne 0
	then
	echo Non root user
	else
	echo Root User
fi
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

