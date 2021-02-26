# IO

input output一切实现的基础

stdio 标准IO

sysio 系统调用IO



标准IO移植性好



## stdio

```c
fopen();
fclose();

fgetc();
fputc();
fgets();
fputs();
fread();
fwrite();

printf();
scanf();

fseek();
ftell();
rewind();

fflush();
```

FILE类型贯穿始终

FILE是结构体struct



```c
FILE *fopen(const char *path, const char *mode);

```









