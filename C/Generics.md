# Generics

Generic function: function that can operate on different types of data.

###  Examples:

ADTs: vector, map

Algorithms: sorting, searching

### Motivation

Unification: no copy/pasting

Performance: optimize once

Convenience: put in standard library



## How

* `void *`
  * 不是说指向void, void不是类型
  * 是可以指向任何类型

* 怎么知道void *指向类型的大小?用于pointer arithmetic



we need to be very careful. Everything we pass into generic function is a pointer to the element.



```c
void qsort(void *arr, size_t nelems, size_t elemsz, int (*cmpfn)(const void*, const void*));
```

sort arr according to cmpfn

* cmpfn: Callback function, a pointer to a function
  * Takes pointers to elements
  * Clients write a function for our array
  * Use typecast to interpret a and b as correct type
  * **qsort** calls cmpfn to determine order

---

```c
int find_max(void *arr, size_t nelems)
{
  int max = arr[0];
  for (int i = 1; i<nelems; i++) {
    if (arr[i] > max) max = arr[i];
  }
  return max;
}
```



Return the maximum value in an array

* Questions/Tasks to convert to generic
* Q1: What type should arr be?
* A1: `void *` 
* Q2 What should find_max return?
* A2: **_ApointerTo_ to the maximum element**
* Q3: How do we get the address of the ith element of arr?
* A3: **Cast arr as char *, add i * nelemsz (bytes)**
* Q4: How do we compare an array element to the max we've seen so far?
* A4: **Pass client-supplied callback function**

```c
void *find_max(void *arr, size_t nelems, size_t elemsz,
              int (*cmpfn)(const void *, const void *))
{
  void *max = arr;
  for (int i = 1; i<nelems; i++)
  {
    void *ith=(char *)arr + i * elemsz;
    if(cmpfun(ith, max)>0) max = ith;
  }
  return max;
}


int cmp_int(const void *va, const void *vb)
{
  int a = *(int *)va, b = *(int *)vb;
  if (a<b) return -1;
  if (a>b) return 1;
  return 0;
}

void test_ints()
{
  int nums[] = {40,19, 23, 45, 12, 45, 23, 32, 45};
  int count = sizeof(nums)/sizeof(nums[0]);
  printf("Array of numbers: ");
  for (int i = 0; i<count; i++)
    printf("%d ", nums[i]);
  printf("\n");
  int max = *(int *)find_max(nums, count, sizeof(nums[0]), cmp_int);
 
}

int cmp_str(const void *va, const void *vb)
{
  // Reminder: Element in array = char *. Pointer to element = char **.
  char *a = *(char **)va, *b = *(char **)vb;
  return strcmp(a, b);
}

void test_strs()
{
  char *str[] = {"pear", "apple", "banana", "mango"};
  int count = sizeof(strs) / sizeof(strs[0]);
  
  printf("Array of strings: ");
  for (int i = 0; i<count; i++)
    	printf("%s ", strs[i]);
  printf("\n");
  char *max = *(char **)find_max(strs, count, sizeof(strs[0]), cmp_str);
  printf("Max of strings: %s\n", max);
}
```

---

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

* Both of these functions copy n bytes from the location pointed to by s2 to the location pointed to by s1, and both return the value of s1. 这两个函数都从s2指向的位置拷贝n个字节到s1指向的位置.
* The difference between the two, as indicated by the keyword restrict, is that memcpy() is free to assume that there is no overlap between the two memory ranges. `memcpy()` 假设两个内存区域之间没有重叠
* memmove()不做这样的假设, 所以拷贝过程类似于先把所有字节拷贝到一个临时缓存区, 然后再拷贝到最终目的地.

---

### Generic Swap

```c
void gswap(void *x, void *y, size_t size_nbytes)
{
  char temp[nbytes];
  memcpy(temp, x, nbytes);
  memcpy(x, y, nbytes);
  memcpy(y, tmp, nbytes);
}

char *s1 = strdup("Standford University");
char *s2 = strdup("Computer Science");
gswap(&s1, &s2, sizeof(char *));
// s1 Computer Science
// s2 Standford University
// if
gswap(s1, s2, sizeof(char *));
// s1 computer university
// s2 stanford science
```



### Generic Array Swap

* swaps the first and the last elem

First thought



