

# C string

## size_t

First, the **stddef.h** header file (included when you include **stdio.h**) defines **size_t** to be whatever the type your system uses for **sizeof**;

`size_t` regularly used for array indexing and loop counting. If the compiler is `32-bit` it would work on `unsigned int`. If the compiler is `64-bit` it would work on `unsigned long long int` also. There for maximum size of `size_t` depending on compiler type.[1][]



| C Declaration | Size (Bytes) |
| :-----------: | :----------: |
|      int      |      4       |
|    double     |      8       |
|     float     |      4       |
|     char      |      1       |
|    char *     |      8       |
|     short     |      2       |
|     long      |      8       |



Under the hood, C represents each `char` as an integer. (it [ASCII](https://www.cs.cmu.edu/~pattis/15-1XX/common/handouts/ascii.html) value)



```c
char uppercaseA = 'A'; // Actually 65
char lowercaseA = 'a'; // 97. 'a'-'A' == 32
char zeroDigit = '0'; // Actually 68

char upperCaseB = 'A' + 1;
int numberLettersInAlphabet = 'z' - 'a' + 1;

// prints out every lowercase letter
for (char ch = 'a'; ch<='z'; ch++) {
  printf("%c", ch);
}
```



### Common ctype.h Functions

| Function     | Description                                      |
| ------------ | ------------------------------------------------ |
| isalpha(ch)  | true if ch is 'a' through 'z' or 'A' through 'Z' |
| islower(ch)  | true if ch is 'a' through 'z'                    |
| isupper(ch)  | true if ch is 'A' through 'Z'                    |
| isspace(ch)  | true if ch is space,  tab, new line, etc         |
| isdigit(ch)  | true if ch is '0' through '9'                    |
| toupper(ch)  | returns uppercase equivalent of a letter         |
| tolower(ch)  | returns lowercase equivalent of a letter         |
| isxdigit(ch) | true if ch is a valid hex digit                  |

**Remeber: these ==return== a char; they cannot modify an existing char!**

## C Strings

<img src="/Users/xuzheng/Projects/notes/CSAPP/C-Strings.assets/image-20201114155921934.png" alt="image-20201114155921934" style="zoom:40%;" />

* C has no dedicated variable type for strings. 
* In C, a tring is represented as an array of characters with a special ending sentinel value.
* `\0` is the **null-terminating character; 

* Strings are not objects. They do not embed addtional information(e.g. string length). 
* `strlen()`in `<string.h>` to calculate string length. 
* **==Caution==**. strlen is O(N) because it must scan the entire string, so we should save the value if we plan to refer to the length later



### How strings are passed into a function?

* It's passed as a `char *`.

### Common string.h Functions

| Function.                  | Description                                                  |
| :------------------------- | :----------------------------------------------------------- |
| `strlen(str)`              | returns the # of chars in a String (before null-terminating character) |
| `strcmp(str1, str2)`       | compares two strings; returns 0 if identical, <0 if str1 come before str2 in alphabet, >0 if str1 comes after str2 in alphabet. |
| `strncmp(str1, str2, n)`   | stops comparing after at most n characters                   |
| `strchr(str, ch)`          | character search: returns a pointer to the first occurrence of `ch` in `str`, Or `NULL` if `ch` was not found in `str` |
| `strrchr(str, ch)`         | find the last occurrence                                     |
| `strstr(haystack, needle)` | string search: returns a pointer to the start of the first occurence of needle in haystack, or **NULL** if needle was not found in **haystack**. |
| `strcpy(dst, src)`         | copies characters in src to dst, including *null-terminating character*. Assumes enough space in *dst*. Strings must not overlap. |
| `strncpy(dst, src, n)`     | stops after at most n chars and DOES NOT add null-terminating char. |
| `strcat(dst, src)`         | concatenate src onto the end of dst. strncat                 |
| strncat(dst, src, n)       | stops concatenating after at most n characters. Always adds a null-terminating character. |
|                            |                                                              |

```c
int strlen(const char *s); 
// Returns the number of characters (excluding the terminating null char *s)
```





```c

size_t strspn(const char *str, const char *accept)
```

returns the length of the maximum initial segment of str that consists entirely of characters from accept.

strspn() 函数返回 str 的最大初始段长度，该段只包含 src 指向的字节串中的字符

> How many places canwe go  in the first string before I encounter a character _not_ in the second string?

```c
char daisy[10];
strcpy(daisy, "Daisy Dog");
int spanLength = strspn(daisy, "aDeoi"); // 3  's'
```

* strcspn(c = "complement") returns the length of the initial part of the first thing which contains only characters not in the second string
  * How many places can we go in the first string before I encounter a character in the second string?





```c
char *strstr(const char *s1, const char *s2);
```

* Returns a pointer to the location of the first occurrence in s1 of the sequence of characters in s2 (excluding the terminating null character); Return NULL if no match is found.

* String search: returns a pointer to the start of the first occurrence of needle in s1(haystack), or NULL if s2(needle) was not found in haystack.

```c
char *strchr(const char *s, int c);
```

* Searches for the first occurrence of c (converted to **char**) in the string pointed to by s; the null character is part of the string; returns a pointer to the first occurrence, or NULL if none is found.

* 在s指向的字符串中搜索c的第一次出现的位置(int c 被转换成char类型). null字符是字符串的一部分; 返回指向首次出现的指针, 如果没有找到返回NULL

strcmp(str1, str2), str1在前, 则<0, str1在后, 则>0.

```c
int cmpResult = strcmp(str1, str2);
if (compResult == 0) {
  // equal
} else if (compResult < 0) {
  // str1 come before str2
} else {
  // str1 comes after str2
}
```



* We must make sure there is enough space in the destination to hold the entire copy, including the null-terminating character.

```c
char str2[6];	// not enough space!
strcpy(str2, "hello, world") // overwrites other memory!
```

* writing past memory bounds is called a "buffer overflow". It can allow for security vulnerabilites!

* If necessary, we can add a null-terminating character ourselves

```c
// copying "hello"
char str2[6]; // room for string and '\0'
strncpy(str2, "hello, world!", 5); // doesn't copy '\0'!
str2[5] = '\0';
```

* 在C中不能直接用 + 去联结字符串要用`strcat` 

```c
char str1[13];         // enough space for strings + '\0'
strcpy(str1, "hello ");
strcat(str1, "world!"); // removes old '\0', adds new '\0' at end 
printf("%s", str1);     // hello world!
```

* `strcat` 和 `strncat` 都会删除旧的`\0` , 在结尾添加新的`\0` 

## `char *` vs. `char[]`

* When you declare a ==char pointer== eqaul to a string literal, the string literal is not stred on the stack. Instead, it's stored in a special area of memory called the "==data segment==". You cannot modify memory in this segment.
* This applies only to **creating new strings with char ***. This does not apply for making a char * that points to an existing stack string.
* 



### printf format

| specifier | description             |
| --------- | ----------------------- |
| %s        | null-terminating string |
| %c        | character               |



### How to print `size_t`

```c
printf("sizeof(i) = %zu\n", sizeof(i));
```



## Pointers to Strings

```c
void skipCSPrefix(char **strPtr) {
  char *prefix = strstr(*strPtr, "CS");
  if (prefix != NULL && prefix == *strPtr) {
    *strPtr += strlen("CS");
  }
}
int main(int argc, char *argv[]) {
  char *myStr = "CS41";
  skipCSPrefix(&myStr);
  printf("%s\n", myStr);
  return 0;
}
```

<img src="/Users/xuzheng/Projects/notes/CSAPP/C-Strings.assets/image-20201115102036824.png" alt="image-20201115102036824" style="zoom:30%;" />



* 传进skipCSPrefix的是myStr的地址, 因为我们想修改的是myStr这个pointer本身的值, 而不是它所指向的字符串

<img src="/Users/xuzheng/Projects/notes/CSAPP/C-Strings.assets/image-20201115103602451.png" alt="image-20201115103602451" style="zoom:40%;" />

## Arrays of Strings

char *stringArray[5]; // space to store 5 char *s

```c
char *stringArray[] = {
  "hello",
  "hi",
  "hey there"
};
```



When an array is passed as a parameter in C, C passes a pointer to the first element of the array. This is what **argv** is in **main**.

void myFunction(char **stringArray) {}

void myFunction(char *stringArray[]) {}









# Resources

* [Learn C programming From Programiz](https://www.programiz.com/c-programming)

[1]: https://stackoverflow.com/questions/2550774/what-is-size-t-in-c#:~:text=size_t%20is%20an%20unsigned%20integer%20data%20type%20which%20can%20assign,you%20can%20run%20the%20programm. "stackoverflow"

