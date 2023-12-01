# 19. Remove Nth Node From End of List

Given the head of a linked list, **remove the nth node from the end of the list** and return its head.

>见Solution 2
(非常漂亮的思路 为什么会想到？快慢指针的差正好是n，漂亮)
## Solution 1(快慢指针,使用dummy)
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        ListNode slow = head;
        ListNode fast = head;
        for(int i=0;i<n;i++){
            fast = fast.next;
        }
        while(fast!=null){
            pre = pre.next;
            slow = slow.next;
            fast = fast.next;
        }
        pre.next = slow.next;
        return dummy.next;
    }
}
```
```python
def removeNthFromEnd(self, head, n):
        dummy = ListNode(-1)
        dummy.next = head
        pre = dummy
        slow = head
        fast = head

        # Move fast pointer n steps ahead
        for _ in range(n):
            fast = fast.next

        # Move fast to the end, maintaining the gap
        while fast:
            pre = pre.next
            slow = slow.next
            fast = fast.next

        # Skip the desired node
        pre.next = slow.next

        return dummy.next
```


优化 删去slow， 直接pre.next.next
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
    
        ListNode fast = head;
        for(int i=0;i<n;i++){
            fast = fast.next;
        }
        while(fast!=null){
            pre = pre.next;
            
            fast = fast.next;
        }
        pre.next = pre.next.next;
        return dummy.next;
    }
}
```

## Solution 2 （不使用dummy）
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode index1 = head, index2 = head;
        for(int i = 0; i < n; i++)  
            index2 = index2.next; //使得index1与index2之间间隔n-1个节点       
        if(index2 == null) 
            return head.next;   //说明删除的是头节点
        while(index2.next != null){      //将index2移至最后一个节点
            index2 = index2.next;
            index1 = index1.next;
        }
        index1.next = index1.next.next;
        return head;
    }
}
// index1.next = index1.next.next; 断链又重新连
// idea: tow pointer, at first point at head, then let p2 move Nth(Nth far from p1, then let p2 reach the end, finally cut off the linklist and rebuil it)
```


```java
public ListNode removeNthFromEnd(ListNode head, int n){
    ListNode dummyNode = new ListNode(0);
    dummyNode.next = head;

    ListNode fastIndex = dummyNode;
    ListNode slowIndex = dummyNode;

    //只要快慢指针相差 n 个结点即可
    for (int i = 0; i < n  ; i++){
        fastIndex = fastIndex.next;
    }

    while (fastIndex.next != null){
        fastIndex = fastIndex.next;
        slowIndex = slowIndex.next;
    }

    //此时 slowIndex 的位置就是待删除元素的前一个位置。
    //具体情况可自己画一个链表长度为 3 的图来模拟代码来理解
    slowIndex.next = slowIndex.next.next;
    return dummyNode.next;
}
```

```python
def removeNthFromEnd(self, head, n):
        dummy = ListNode(-1)
        dummy.next = head
        pre = dummy
        slow = head
        fast = head

        # Move fast pointer n steps ahead
        for _ in range(n):
            fast = fast.next

        # Move fast to the end, maintaining the gap
        while fast:
            pre = pre.next
            slow = slow.next
            fast = fast.next

        # Skip the desired node
        pre.next = slow.next

        return dummy.next

```