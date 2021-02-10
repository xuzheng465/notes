## memcpy() and memmove() from string.h Library

```c
void *memcpy(void * restrict s1, const void * restrict s2, size_t n);
void *memmove(void *s1, const void *s2, size_t n);
```



```c
void * memcpy(void *dest, const void *src, size_t len)
{
  char *d = dest;
  const char *s = src;
  while (len--)
    *d++ = *s++;
  return dest;
}
```

* `memcpy` is a function that copies a specified amount of bytes at one address to another address



* Both of these functions copy n bytes from the location pointed to by s2 to the location pointed to by s1, and both return the value of s1. 
  * 这两个函数都从s2指向的位置拷贝n个字节到s1指向的位置.
* The difference between the two, as indicated by the keyword restrict, is that memcpy() is free to assume that there is no overlap between the two memory ranges. 
  * `memcpy()` 假设两个内存区域之间没有重叠
* `memmove()`不做这样的假设, 所以拷贝过程类似于先把所有字节拷贝到一个临时缓存区, 然后再拷贝到最终目的地.





