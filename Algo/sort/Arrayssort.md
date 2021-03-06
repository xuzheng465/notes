## Arrays.sort

查看了下`Arrays.sort`的源码，主要采用**TimSort**算法, 大致思路是这样的：

1 元素个数 < 32, 采用二分查找**插入排序**(Binary Sort)
2 元素个数 >= 32, 采用归并排序，归并的核心是分区(Run)
3 找连续升或降的序列作为分区，分区最终被调整为升序后压入栈
4 如果分区长度太小，通过二分插入排序扩充分区长度到分区最小阙值
5 每次压入栈，都要检查栈内已存在的分区是否满足合并条件，满足则进行合并
6 最终栈内的分区被全部合并，得到一个排序好的数组

Timsort的合并算法非常巧妙：

1 找出左分区最后一个元素(最大)及在右分区的位置
2 找出右分区第一个元素(最小)及在左分区的位置
3 仅对这两个位置之间的元素进行合并，之外的元素本身就是有序的





java1.8中的排序:

在元素小于47的时候用插入排序，

大于47小于286用双轴快排，

大于286用timsort归并排序，

并在timesort中记录数据的连续的有序段的的位置，若有序段太多，也就是说数据近乎乱序，则用双轴快排，当然快排的递归调用的过程中，若排序的子数组数据数量小，用插入排序。