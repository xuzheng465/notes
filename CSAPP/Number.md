# Number



## Number representation



| C Declaration | Size (Bytes) |
| :-----------: | :----------: |
|      int      |      4       |
|    double     |      8       |
|     float     |      4       |
|     char      |      1       |
|    char *     |      8       |
|     short     |      2       |
|     long      |      8       |



32-bit

2^32^ bytes of addressable memory

This equals 4 Gigabytes, meaning that ==32-bit computers could have at most **4GB** of memory(RAM)==



64-bit pointers store a memory address from 0 to 2^64^ -1, equaling 2^64^ bytes of addressable memory. 

This equals **16 Exabytes** , meaning that ==64-bit computers could have at most **1024\*1024\*1024** GB of memory (RAM) !==



## Unsigned Integers

* The range of an unsigned number is 0--> 2^w^ - 1, where w is the number of bits.
  * a 32-bit integer can represent 0 to 2^32^ - 1(4,294,967,295)
* 