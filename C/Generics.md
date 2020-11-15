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
* A2: 
* Q3: How do we get the address of the ith element of arr?
* A3:
* Q4: How do we compare an array element to the max we've seen so far?
* A4:

