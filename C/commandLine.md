* `main()` 函数有两个版本，一个有命令行参数，一个没有
* 命令行参数通过两个变量传递给`main()`函数，一个是参数的计数`argc`, 另一个是指针（指向参数字符串）数组
* 命令行选项是以`"-"`开头的命令行参数
* `getopt()`函数会帮助你处理命令行选项
* 为了定义有效的选项，可以传给`getopt()`一个字符串，例如`ae:`
* 选项之后的“：” （冒号）表示该选项需要接收一个参数
* `getopt()`会用`optarg`变量记录选项参数
* 读取完全部的选项后应该用optind变量跳过它们



```c

#include <stdio.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
    char *delivery = "";
    int thick = 0;
    int count = 0;
    char ch;

    while ((ch = getopt(argc, argv, "d:t")) != EOF)
    {
        switch (ch)
        {
        case 'd':
            delivery = optarg;
            break;
        case 't':
            thick = 1;
            break;
        default:
            fprintf(stderr, "Unknown option: '%s'\n", optarg);
            return 1;
        }
    }
    argc -= optind;
    argv += optind;

    if (thick)
        puts("Thick crust.");
    if (delivery[0])
        printf("To be delivered %s.\n", delivery);
    puts("Ingredients: ");

    for (count = 0; count < argc; count++)
        puts(argv[count]);

    return 0;
}

```

