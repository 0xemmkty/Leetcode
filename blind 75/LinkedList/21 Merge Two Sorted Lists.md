# 21. Merge Two Sorted Lists

You are given the heads of **two sorted linked lists** list1 and list2.

Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

## Solution 1 (recrusion)
复杂度分析

如何计算递归的时间复杂度和空间复杂度呢？ 力扣对此进行了 详细介绍 ，其中时间复杂度可以这样计算：

给出一个递归算法，其时间复杂度 O(T) 通常是递归调用的数量（记作R） 和计算的时间复杂度的乘积（表示为 O(s)）的乘积：O(T)=R∗O(s)

时间复杂度：O(m+n)。

m，n 为l1 和 l2
​
  的元素个数。递归函数每次去掉一个元素，直到两个链表都为空，因此需要调用 R=O(m+n) 次。而在递归函数中我们只进行了 next 指针的赋值操作，复杂度为 O(1)，故递归的总时间复杂度为 O(T)=R∗O(1)=O(m+n) 。

空间复杂度：O(m+n)。**

对于递归调用 self.mergeTwoLists()，当它遇到终止条件准备回溯时，已经递归调用了m+n 次，使用了m+n 个栈帧，故最后的空间复杂度为 O(m+n)。


```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null)return l2;
        if(l2 == null)return l1;
        ListNode head;
        if(l1.val < l2.val)
        {
            head = l1;
            head.next = mergeTwoLists(l1.next,l2);
        }
        else
        {
            head = l2;
            head.next = mergeTwoLists(l1,l2.next);
        }
        return head;
    }
}
// 谁小谁是head，下次比较是那个小的值的head
```

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        else if (l2 == null) {
            return l1;
        }
        else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }

    }
}


```
