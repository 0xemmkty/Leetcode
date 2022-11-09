# 2. Add Two Numbers

>You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

>You may assume the two numbers do not contain any leading zero, except the number 0 itself.
```
2->4->3
5->6->4

7->0->8
```
```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```
## Solution 1 （循环）
时间复杂度：O(max(M,N))，M 和 N 分别是链表 a 和 b 的长度；
空间复杂度：O(1)，只使用了常数的空间。
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // tell the pointer and the node
        // dummy is a pointer which point to the new node,
        // cur is a pointer which point to the dummy
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        int carry = 0;
        while (l1 != null || l2 != null || carry != 0) {
            // if null. replace with 0
            int i1 = l1 != null ? l1.val : 0;
            int i2 = l2 != null ? l2.val : 0;
            int add = i1 + i2 + carry;
            carry = add >= 10 ? 1 : 0;
            add = add >= 10 ? add - 10 : add;
            // move l1, l2, cur
            // also a new node
            cur.next = new ListNode(add);
            cur = cur.next;
            if (l1 != null)
                l1 = l1.next;
            if (l2 != null)
                l2 = l2.next;
        }
        return dummy.next;
    }
}

```
carry

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0); // creating an dummy list
        ListNode curr = dummy; // intialising an pointer
        int carry = 0; // intialising our carry with 0 intiall
        // while loop will run, until l1 OR l2 not reaches null OR if they both reaches null. But our carry has some value in it. 
		// We will add that as well into our list
        while(l1 != null || l2 != null || carry == 1){
            int sum = 0; // intialising our sum
            if(l1 != null){ // adding l1 to our sum & moving l1
                sum += l1.val;
                l1 = l1.next;
            }
            if(l2 != null){ // adding l2 to our sum & moving l2
                sum += l2.val;
                l2 = l2.next;
            }
            sum += carry; // if we have carry then add it into our sum
            carry = sum/10; // if we get carry, then divide it by 10 to get the carry
            ListNode node = new ListNode(sum % 10); // the value we'll get by moduloing it, will become as new node so. add it to our list
            curr.next = node; // curr will point to that new node if we get
            curr = curr.next; // update the current every time
        }
        return dummy.next; // return dummy.next bcz, we don't want the value we have consider in it intially!!
    }
}
```

## Solution 2 (递归)
```
递归函数定义：addTwoNumbers 表示将两个链表 l1 和 l2 相加得到的新链表； 递归终止条件：如果 l1 和 l2 有一个为空，则返回另外一个。 递归函数内容：

把两个链表节点的值相加（结果记为 add ）。把 add 模 10 作为当前的链表节点的值。
把两个链表的 next 节点相加。（注意：如果当前相加的结果 add>=10，需要把 next 节点相加得到的结果 + 1。）
​ 递归解法妙在天然地处理好了两个链表不一样长、最终相加结果有进位的情况。
```
```java
// very beautiful!!!!
// addnumber function :
// 1. add two node
// 2. add carry
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if (l1 == null)
            return l2;
        if (l2 == null)
            return l1;
        int add = l1.val + l2.val;
        ListNode res = new ListNode(add % 10);
        res.next = addTwoNumbers(l1.next, l2.next);
        if (add >= 10)
        // how to add 1, ListNode(1)
            res.next = addTwoNumbers(res.next, new ListNode(1));
        return res;
    }
}
```