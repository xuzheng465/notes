# Stack and Heap



## Stack



* The stack is the place where all local variables and parameters live for each function. (栈是每个函数的所有局部变量和参数所在的地方)
* A function's stack "frame" goes away when the function returns.( 当函数返回时, 栈帧就被~~回收/删除~~了)
  * C does not clear out memory when a function's frame is removed. Instead, it just marks that memory as usable for the next function call.
* The stack grows **downwards** when a new function is called and **shrinks** upwards when the function is finished

<img src="/Users/xuzheng/Projects/notes/C/Stack and Heap.assets/image-20201115193516438.png" alt="image-20201115193516438" style="zoom:50%;" />

## Heap

* Heap is a part of memory that you can manage yourself.
* This kind of memory only goes away when you delete it your self.
* Heap grows **upwards** as memory is allocated.

### malloc

```c
void *malloc(size_t size);
```

* This function returns a pointer to the starting address of the new memory. It doesn't know or care whether it will be used as an array, a single block of memory.
* void * means a pointer to generic memory. You can set another pointer equal to it without any casting.
* if `malloc` returns NULL, then there wasn't enough memory for this request.

```c
char *create_string(char ch, size_t size) {
  char *rt = malloc(sizeof(char) * (size+1));
  assert(rt != NULL);						// always assert with the heap
  for (int i = 0; i<size; i++) {
    rt[i] = ch;
  }
  rt[size]='\0';
  return rt;
}
```

* ⚠️
  * 用一个指针去储存malloc返回的地址
  * malloc的参数是将要分配的字节数
  * always ==assert== with heap

### calloc

```c
void *calloc(size_t nmemb, size_t size);
// calloc is like malloc that zeros out the memory for you 
```

```c
// allocate and zero 20 units
int *score = calloc(20, sizeof(int));

// alternate (but slower)
int *scores = malloc(20*sizeof(int));
for (int i = 0;i<20;i++) scores[i] = 0;
```

### strdup

```c
char *strdup(char *s);
```

 returns a null-terminated, heap-allocated string with the provided text/ 

```c
char *str = strdup("Hello, world!"); // on heap
str[0] = 'h';
```



### free

```c
void free(void *ptr);
```

* use free command and pass in the starting addr on the heap for the memory you no longer need.
* each memory block should only be freed **once**
* 

### realloc

```c
void *realloc(void *ptr, size_t size);
```

* The realloc function takes an existing allocation pointer and enlarges to a new request size. It returns the new pointer.
* If there is enough space after the existing memory block on the heap for the new size, **realloc** simply adds that space to the allocation.
* If there is not enough space, realloc moves the memory to a larger location, ***frees the old memory*** for you, and returns a pointer to the new location.

```c
char *str = strdup("Hello");
assert(str != NULL);

// want to make str longer to hold "Hello World!"
char *addition = " world!";
str = realloc(str, strlen(str) + strlen(addition) + 1);
assert(str!=NULL);
strcat(str, addition);
printf("%s", str);
free(str)
```



* realloc只接受由`malloc` 返回的指针.
* 不要向堆分配的内存中间传递指针
* 不要向栈内存传入指针

