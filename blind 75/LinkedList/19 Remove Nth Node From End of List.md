19. Remove Nth Node From End of List

Given the head of a linked list, **remove the nth node from the end of the list** and return its head.

>见Solution 2

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