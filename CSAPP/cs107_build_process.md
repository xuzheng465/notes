# Build process

*  预编译: 宏定义展开, 头文件展开, 条件编译, 去掉注释 
  * `gcc -E source.c -o pre.i`

* 编译: 检查语法, 将源代码变成汇编语言
  *   `gcc -S pre.i -o ass.s`

* 汇编: 将汇编语言编程机器语言(二进制语言)
  *  `gcc -c ass.s -o bin.o`

* 链接: 链接外部的库
  *  `gcc bin.o -o execu`



## Steps 

1. Preprocessor (`gcc -E`) 

   * Input: binky.c (C source code) 

   * Output: binky.i (C source code) 

   * Purpose: Handles #include, #define

2. Compiler (gcc -S) 

   * Input: binky.i (C code) 

   * Output: binky.s (Assembly code, text) 

   * Purpose: 

\- Assembler (gcc -c) 

\- Input: binky.s (Assembly) 

\- Output: binky.o (Machine code, binary, "object file") 

\- Purpose: 

\- Linker (gcc [no flag]) 

\- Input: binky.o, winky.o, Iibdinky.a (multiple object files, 

\- Output: binky (executable program) 

\- Purpose: 

.0 file vs. executable 

libraries)

## Error

* Preprocessor
  * WILL NOT handle syntax errors in ***#define***