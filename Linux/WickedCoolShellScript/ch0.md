## 脚本#1

```bash
#! /bin/bash
# inpath -- 验证指定程序是否有效，或者能否在PATH目录列表中找到

in_path()
{
	# 尝试在环境变量PATH中找到给定的命令。如果找到返回0；
	# 如果没找到，则返回1.注意，该函数会临时修改IFS， (internal field separator)(内部字段分隔符)
	# 不过在函数执行完毕时会将其恢复原状
	cmd=$1	ourpath=$2	result=1
	oldIFS=$IFS	IFS=":"
	for directory in $ourpath
	do
		if [ -x $directory/$cmd ] ; then
			result=0
		fi
	done
	
	IFS=$oldIFS
	return $result
}

checkForCmdInPath()
{
	var=$1
    if [ "$var" != "" ]; then
        if [ "${var:0:1}" = "/" ]; then
            if [ ! -x $var ]; then
                return 1
            fi
        elif ! in_path $var "$PATH"; then
            return 2
        fi
    fi
	
}
```

**`${varname:start:size}`**

变量切分语法`${var:0:1}` 是一种可以在字符串中指定子串的简写法，

从偏移处开始，按照给定长度截取（如果没有 提供长度，则返回余下的全部字符串）。

`${var:10}` 将会从第11个字符开始返回变量$var余下的值

`${var:10:5}` 则返回第11个到第15个字符。



可以将表达式`${var:0:1}` 换成更为负载的 `${var%${var#?}}`. 后者是POSIX的变量切分写法。

`${var#?}` 会提取变量var中除第一个字符之外的其余所有内容，

`#` 表示删除指定模式的第一处匹配

`?` 是正则表达式，只匹配单个字符。

`${var%pattern}` 会产生一个子串，其值为将**指定模式**从变量var中删除后所剩下的部分。（应该从变量var的右侧开始，删除指定的模式）

```bash
var=something wicked this way comes...
echo $var
# something wicked this way comes...
echo ${var#?}
# omething wicked this way comes...
echo ${var%${var#?}}
# s
```



## 脚本#2 验证输入：仅限字母数字

```bash
#! /bin/bash
# validAlphaNum -- 确保输入内容仅限于字母和数字

validAlphaNum()
{
	# 返回值：如果输入内容全部都是字母和数字，那么返回0；否则，返回1
	# 删除所有不符合要求的字符
	validchars="$(echo $1 | sed -e 's/[^[:alnum:]]//g')"
	
	if [ "$validchars" = "$1" ] ; then
		return 0
	else
		return 1
	fi
}

# 主脚本开始 -- 如果要将该脚本包含到其他脚本之内，那么删除或注释掉本行以下的所有内容。 
# ================= 
/bin/echo -n "Enter input: " 
read input
# Input validation
if ! validAlphaNum "$input" ; then
	echo "Your input must consist of only letters and numbers." >&2
	exit 1
else
	echo "Input is valid."
fi
exit 0
```

&2 Standard Error

`[:alnum:]` 是POSIX的正则表达式的简写，“删除有问题的字符，然后看看剩下什么”

如果除了大写字母之外，还需要空格、逗号和点号

```bash
sed 's/[^[:upper:],.]//g'
```



## 脚本#3 规范日期格式

shell脚本 normdate

```bash
#! /bin/bash
# normdate -- 将月份规范成3个字母，首字母大写
# 如果没有错误将以0值退出。
monthNumToName() {
	# 将变量month设置为相应的值
	case $1 in
	1 ) month="Jan" ;;	2 ) month="Feb" ;;
	3 )	month="Mar"	;;	4 ) month="Apr"	;;
	5 ) month="May"	;;	6 ) month="Jun"	;;
	7 ) month="Jul"    ;;  8 ) month="Aug"    ;;
	9 ) month="Sep"    ;;  10) month="Oct"    ;;
  11) month="Nov"    ;;  12) month="Dec"    ;;
  * ) echo "$0: Unknown numeric month value $1" >&2; exit 1
  esac
  
  return 0

}

# BEGIN MAIN SCRIPT DELETE BELOW THIS LINE IF YOU WANT TO 
#   INCLUDE THIS IN OTHER SCRIPTS.
# =================
# Input validation
if [ $# -ne 3 ] ; then
 echo "Usage: $0 month day year" >&2
 echo "Formats are August 3 1962 and 8 3 1962" >&2
 exit 1
fi
if [ $3 -le 99 ] ; then	
 echo "$0: expected 4-digit year value." >&2; exit 1
fi

Is the month input format a number?
if [ -z $(echo $1|sed 's/[[:digit:]]//g')  ]; then
 monthNumToName $1
else
  Normalize to first three letters, first upper-, rest lowercase.
 month="$(echo $1|cut -c1|tr '[:lower:]' '[:upper:]')"
 month="$month$(echo $1|cut -c2-3 | tr '[:upper:]' '[:lower:]')"
fi

echo $month $2 $3

exit 0

```

















