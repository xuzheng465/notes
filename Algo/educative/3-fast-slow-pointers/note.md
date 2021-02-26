# Fast & Slow pointers





快慢指针方法，也被称为兔子和乌龟算法，是一种使用两个指针以不同速度在**数组**（或**序列**/**LinkedList**）中移动的**指针**算法。这种方法在处理**循环**LinkedLists或数组时相当有用。

通过以不同的速度移动（比如说，在一个循环LinkedList中），`算法证明了两个指针一定会相遇`。一旦两个指针都处于循环循环中，快指针就应该抓住慢指针。

用这种技术解决的一个著名问题是`Finding a cycle in a LinkedList`。让我们跳到这个问题来理解**快慢模式**。



leetcode 141 环形链表



Problem 1: 给定一个有环的LinkedList的头部，找到长度

解决办法。我们可以使用上面的解决方案来寻找LinkedList中的循环。一旦快慢指针相遇，我们可以保存慢指针，然后用另一个指针迭代整个循环，直到再次看到慢指针，从而找到循环的长度。



## 找到环的起始node

**Problem** 2: Given the head of a **Singly LinkedList** that contains a cycle, write a function to find the **starting node of the cycle**.

**Solution**：

If we know the length of the **LinkedList** cycle, we can find the start of the cycle through the following steps:

1. Take two pointers. Let’s call them `pointer1` and `pointer2`.
2. Initialize both pointers to point to the start of the LinkedList.
3. We can find the length of the LinkedList cycle using the approach discussed in [LinkedList Cycle](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6556337280385024). Let’s assume that the length of the cycle is ‘K’ nodes.
4. Move `pointer2` ahead by ‘K’ nodes.
5. Now, keep incrementing `pointer1` and `pointer2` until they both meet.
6. As `pointer2` is ‘K’ nodes ahead of `pointer1`, which means, `pointer2` must have completed one loop in the cycle when both pointers meet. Their meeting point will be the start of the cycle.

1. 先找到环的长度 比如K
2. 取两个指针
3. 两个指针都指向链表的头部
4. 把`ptr2`向前移动`K`个节点
5. 现在，同时增加两个指针（每次动一步），直到他们相遇
6. 因为`ptr2`在ptr1前面`K`个节点，这表示ptr2必须完成环的一圈，两点才相遇。他们的相遇点就是环的起始点

TC：O(N)

SC:  O(1)



## Happy Number

==Leetcode 202==

### Problem

它们所有数字的平方之和，将会让我们得到`1`

Input 23

Output： true

2^2^ + 3^2^ = 4 + 9 = 13

1^2^ + 3^2^ = 1 + 9 = 10

1^2^ + 0^2^ = 1 + 0 = 1



### solution:

如果 n 是一个快乐数，即没有循环，那么快跑者最终会比慢跑者先到达数字 1。

如果 n 不是一个快乐的数字，那么最终快跑者和慢跑者将在同一个数字上相遇。



## middle of linkedlist

### Problem:

Given the head of a **Singly LinkedList**, write a method to return the **middle node** of the LinkedList.



If the total number of nodes in the LinkedList is even, return the second middle node.

### Solution

We can use the **Fast & Slow pointers** method such that the fast pointer is always twice the nodes ahead of the slow pointer. This way, when the fast pointer reaches the end of the LinkedList, the slow pointer will be pointing at the `middle node`.



TC: O(N)

SC: O(1)



## Palindrome LinkedList



### Problem:

Given the head of a **Singly LinkedList**, write a method to check if the **LinkedList is a palindrome** or not.

**Example 1:**

```
Input: 2 -> 4 -> 6 -> 4 -> 2 -> null
Output: true
```

**Example 2:**

```
Input: 2 -> 4 -> 6 -> 4 -> 2 -> 2 -> null
Output: false
```

### Solution 1:

1. 先用快慢指针的方法找到LinkedList的中点。
2. 一旦有了中点，我们会反转第二半部分
3. 比较第一部分，和反转后的第二部分
4. 最后，再反转被反转过的LinkedList回原有形式

TC: O( N ) , N是指链表的大小

SC: O( 1 )



### Solution 2











### Problem:





### Solution:







### Problem:





### Solution:







### Problem:





### Solution:





