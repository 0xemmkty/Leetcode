# 203. Remove Linked List Elements

Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

节点删除 实际上就是断掉next原本连接的，将next连向新的
我们可以用新建head节点通过移动进行“遍历”，但是就会有一个问题，即如何删除原链表的第一个节点呢?
所以我们也可以用另一个办法，即在原链表第一个节点前新建一个虚拟节点dummy，这样一来删除每个节点都可以用同样的方式了

## Solution 1（添加dummy节点）
```java
/**
 * 添加虚节点方式
 * 时间复杂度 O(n)
 * 空间复杂度 O(1)
 * @param head
 * @param val
 * @return
 */
public ListNode removeElements(ListNode head, int val) {
    if (head == null) {
        return head;
    }
    // 因为删除可能涉及到头节点，所以设置dummy节点，统一操作
    ListNode dummy = new ListNode(-1, head);
    ListNode pre = dummy;
    ListNode cur = head;
    while (cur != null) {
        if (cur.val == val) {
            // pre 的移动是分情况的
            pre.next = cur.next;
        } else {
            pre = cur;
        }
        cur = cur.next; // 不论什么情况下 cur都会往后移
    }
    return dummy.next; // dummy是我们设置的虚拟节点，因此dummy.next就是我们需要的
}
```


## other solution
```java
/**
 * 不添加虚拟节点and pre Node方式
 * 时间复杂度 O(n)
 * 空间复杂度 O(1)
 * @param head
 * @param val
 * @return
 */
public ListNode removeElements(ListNode head, int val) {
    while(head!=null && head.val==val){
        head = head.next;
    }
    ListNode curr = head;
    while(curr!=null){
        while(curr.next!=null && curr.next.val == val){
            curr.next = curr.next.next;
        }
        curr = curr.next;
    }
    return head;
}
```

```java
/**
 * 不添加虚拟节点方式
 * 时间复杂度 O(n)
 * 空间复杂度 O(1)
 * @param head
 * @param val
 * @return
 */
public ListNode removeElements(ListNode head, int val) {
    // 将head的情况分类
    while (head != null && head.val == val) {
        head = head.next;
    }
    // 已经为null，提前退出
    if (head == null) {
        return head;
    }
    // 已确定当前head.val != val
    ListNode pre = head;
    ListNode cur = head.next;
    while (cur != null) {
        if (cur.val == val) {
            pre.next = cur.next;
        } else {
            pre = cur;
        }
        cur = cur.next;
    }
    return head;
}
```