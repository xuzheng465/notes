## Introduction



Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.



给定一个排过序的数组和一个 target sum，找到数组中的一堆其和为target sum

两个指针，一个指着数组第一个元素，另一个指着数组末尾元素。

看他们俩的和：

1. 如果这两个数的和大于target sum，这意味着我们需要小一点的数，所以就要减小末端指针
2. 如果这两个数的和小于target sum， 这意味着我们需要大一点的数，增加首端指针。



