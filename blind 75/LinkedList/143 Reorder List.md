# 143. Reorder List

>You are given the head of a singly linked-list. The list can be represented as:
```
L0 → L1 → … → Ln - 1 → Ln
```
>Reorder the list to be on the following form:
```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```
>You may not modify the values in the list's nodes. Only nodes themselves may be changed.

## Solution 1 Storage
The disadvantage of a linked list is that it cannot be stored randomly, so when we want to take the last element, we have to iterate through it from the beginning, which is time-consuming. The second time we fetch the last element, we have to iterate through it again.

So let's start with a simple and brutal idea, store the linked list in a linear list, and then use double pointers to fetch the elements from the beginning and the end in turn.

Translated with www.DeepL.com/Translator (free version)
[解法一 存储
链表的缺点就是不能随机存储，当我们想取末尾元素的时候，只能从头遍历一遍，很耗费时间。第二次取末尾元素的时候，又得遍历一遍。

所以先来个简单粗暴的想法，把链表存储到线性表中，然后用双指针依次从头尾取元素即可。]
思路直接，但是耗时

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public void reorderList(ListNode head) {
    if (head == null) {
        return;
    }
    //存到 list 中去
    List<ListNode> list = new ArrayList<>();
    while (head != null) {
        list.add(head);
        head = head.next;
    }
    //头尾指针依次取元素
    int i = 0, j = list.size() - 1;
    while (i < j) {
        list.get(i).next = list.get(j);
        i++;
        //偶数个节点的情况，会提前相遇
        if (i == j) {
            break;
        }
        list.get(j).next = list.get(i);
        j--;
    }
    list.get(i).next = null;
}


}
```
## Solution 2
见图片143
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
   public void reorderList(ListNode head) {

    if (head == null || head.next == null || head.next.next == null) {
        return;
    }
    int len = 0;
    ListNode h = head;
    //求出节点数
    while (h != null) {
        len++;
        h = h.next;
    }

    reorderListHelper(head, len);
}

private ListNode reorderListHelper(ListNode head, int len) {
    if (len == 1) {
        ListNode outTail = head.next;
        head.next = null;
        return outTail;
    }
    if (len == 2) {
        ListNode outTail = head.next.next;
        head.next.next = null;
        return outTail;
    }
    //得到对应的尾节点，并且将头结点和尾节点之间的链表通过递归处理
    ListNode tail = reorderListHelper(head.next, len - 2);
    ListNode subHead = head.next;//中间链表的头结点
    head.next = tail;
    ListNode outTail = tail.next;  //上一层 head 对应的 tail
    tail.next = subHead;
    return outTail;
}


}
```

## Solution 3
```
1 -> 2 -> 3 -> 4 -> 5 -> 6
第一步，将链表平均分成两半
1 -> 2 -> 3
4 -> 5 -> 6
    
第二步，将第二个链表逆序
1 -> 2 -> 3
6 -> 5 -> 4
    
第三步，依次连接两个链表
1 -> 6 -> 2 -> 5 -> 3 -> 4
```
第一步找中点的话，可以应用 19 题 的方法，快慢指针。快指针一次走两步，慢指针一次走一步，当快指针走到终点的话，慢指针会刚好到中点。如果节点个数是偶数的话，slow 走到的是左端点，利用这一点，我们可以把奇数和偶数的情况合并，不需要分开考虑。

第二步链表逆序的话，在 第 2 题 讨论过了，有迭代和递归的两种方式，迭代的话主要利用两个指针，依次逆转。

第三步的话就很简单了，两个指针分别向后移动就可以。


```java
public void reorderList(ListNode head) {
    if (head == null || head.next == null || head.next.next == null) {
        return;
    }
    //找中点，链表分成两个
    ListNode slow = head;
    ListNode fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    ListNode newHead = slow.next;
    slow.next = null;
    
    //第二个链表倒置
    newHead = reverseList(newHead);
    
    //链表节点依次连接
    while (newHead != null) {
        ListNode temp = newHead.next;
        newHead.next = head.next;
        head.next = newHead;
        head = newHead.next;
        newHead = temp;
    }

}

private ListNode reverseList(ListNode head) {
    if (head == null) {
        return null;
    }
    ListNode tail = head;
    head = head.next;

    tail.next = null;

    while (head != null) {
        ListNode temp = head.next;
        head.next = tail;
        tail = head;
        head = temp;
    }

    return tail;
}


```

