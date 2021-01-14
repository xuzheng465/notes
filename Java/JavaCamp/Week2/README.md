## 1. JDK 内置命令行工具

| gongju         | des                             |
| -------------- | ------------------------------- |
| jps / jinfo    | 查看java进程                    |
| jstat          | 查看JVM内部gc相关信息           |
| jmap           | 查看heap或类占用空间统计        |
| jstack         | 查看线程信息                    |
| jcmd           | 执行JVM相关分析命令（整合命令） |
| jrunscript/jjs | 执行js命令                      |

### jps

```bash
jps -l
```

显示运行jar包



```bash
jps -mlv
```

列出java进程 还有 参数 都打印出来

买个lv的包

mlv



### jstat

| options | des                                   |
| ------- | ------------------------------------- |
| -class  | 类加载（Class Loader）的信息统计      |
| -gcutil | GC相关区域的使用率（utilization）统计 |
| -gc     | GC相关的堆内存信息                    |
|         |                                       |



```bash
jstat -gc pid 1000 1000
```

1000 1000

每一秒打印一次， 打印1千次





### jstack

-l 长列表模式。 将线程相关的locks信息一起输出，比如持有的锁，等待的锁

kill -3 

当前进程打出来

kill -9 pid

强杀一个进程

### jmap

| options | des                            |
| ------- | ------------------------------ |
| -heap   | 打印堆内存的配置和使用信息     |
| -histo  | 看哪些类占用的空间最多，直方图 |
| -dump   | Dump 堆内存                    |



jmap -heap pid

jmap -histo pid



### jcmd



jcmd pid VM.version

jcmd pid VM.flags

jcmd pid VM.command_line

jcmd pid VM.system_properties

jcmd pid Thread.print

jcmd pid GC.class_histogram

jcmd pid GC.heap_info







jstat jmap jstack

jmc



