C语言学习建议：

1. 概念的正确性
2. 动手能力
3. 阅读优秀的程序段
4. 大量练习，面试题



hello.c



c源文件-->预处理（-E) --> 编译  --> 汇编 -->   -->



#### 预处理：

`gcc -E hello.c > hello.i`

#### 编译

`gcc -S hello.i`

默认产生`hello.s`



#### 汇编

`gcc -c hello.s`

生成`hello.o` 目标文件



