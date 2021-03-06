- 数组变量可以被用作指针
- 数组变量指向数组中第一个元素
- ==如果把函数参数声明为数组，它会被当作指针处理==。
- sizeof运算符返回某条数据占用空间的大小
- `sizeof(pointer)` 在32位操作系统中返回4，在64位操作系统中返回8 



> :heavy_exclamation_mark: sizeof 是一个运算符
>
> 编译器会把`运算符`编译为一串指令；而当程序调用`函数`时，会跳到一段独立的代码中执行.
>
> 所以编译器可以在**编译时**确定存储空间的大小



### 指针退化

* `数组变量`不能指向其他地方。
* 当创建`指针变量`时，计算机会为它分配4或8字节的存储空间。
* 当创建`数组`时，计算机会为`数组`分配存储空间，但不会为`数组变量`分配任何空间。
* 编译器仅在出现它的地方把它替换成数组的起始地址。
* 由于计算机没有为`数组变量`分配空间，也就不能把它指向其他地方。



* 把数组赋给指针变量，`指针变量`只会包含`数组`的地址信息，而对数组的长度一无所知, 相当于指针丢失了一些信息，此被称为**退化**。
* 只要把数组传递给函数，数组免不了退化为指针。



### 字符串字面值不能更新

指向字符串字面值的指针变量不能用来修改字符串的内容：

```c
char *cards = "JQK";
```

如果用字符串字面值创建一个**数组**，就可以修改了：

```c
char cards[] = "JQK";
```

这是由C语言使用`存储器`的方式决定的



第一种情况`JQK`是常数值，放到常量存储区，这部分存储器是**只读**的。



**栈**是存储器中计算机用来保存局部变量的部分，局部变量也就是位于函数内部的变量，cards变量就在这个地方。

cards变量将会保存字符串字面值`“JQK”`的地址。为了防止修改，字符串字面值通常保存在只读存储器中。

> 一切字符串皆为数组,但是在旧代码中,cards只是一个指针,而在新代码中,它是一个数组。假如你声明了一个名为cards的数组,然后把它设置成字符串字面值,cards数组就成为了一个全新副本。cards不再只是一个指向字符串字面值的指针,而是一个崭新的数组,里面保存了字符串字面值的最新副本。



```c

int my_function() {
	char cards[] = "JQK";
}
// cards是数组

```

如果是普通的变量声明，cards就是一个数组，必须立即赋值。

但如果cards以函数参数的形式声明，那么cards就是一个指针：



```c
void stack_deck(char cards[])
{
  
}
// equals
void stack_deck2(char * cards)
{
  ...
}
```

以上连个函数等价。

```c

```

























