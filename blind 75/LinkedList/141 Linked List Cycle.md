# 141. Linked List Cycle

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

## Solution 1 (快慢指针)

when talk about linklist, it very common to think of double pointer.
in double pointer, there is a special one, called fast-slow pointer.
so, how can we determind if there is a ring or not?
Use fast-slow pointer!!!
you can think that if there is a ring, after enough loop, these two pointer will come across each other.

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
    ListNode fast = head, slow = head;
    while(fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if(fast == slow) { // 在指针移动后再比较，排除初始都指向头结点的情况
            return true;
        }
    }
    return false;
}
}
// very straighntforward idea!
//remember to compare after movement to aviod head point.
```