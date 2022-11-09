# 445. Add Two Numbers II 
(review leetcode 2, which most significant num is on the back, but in this question, we need to reverse link or use stack and loop)

You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Follow up:

What if you cannot modify the input lists? In other words, **reversing the lists is not allowed.**

Example:

Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7

## Solution 1 (stack + loop)
```java
// compare to the leetcode 2, this answer add more line for stack and push method. 
// same idea in total.
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<Integer> st1 = new Stack();
        Stack<Integer> st2 = new Stack();
        while (l1 != null) {
            st1.push(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            st2.push(l2.val);
            l2 = l2.next;
        }
        int carry = 0;
        ListNode dummy = new ListNode(0);
        while (!st1.empty() || !st2.empty() || carry > 0) {
            int adder1 = st1.empty() ? 0 : st1.pop();
            int adder2 = st2.empty() ? 0 : st2.pop();
            int sum = adder1 + adder2 + carry;
            carry = sum >= 10 ? 1 : 0;
            sum = sum >= 10 ? sum - 10 : sum;
            ListNode cur = new ListNode(sum);
            cur.next = dummy.next;
            dummy.next = cur;
        }
        return dummy.next;
    }
}


```
复杂度分析：

时间复杂度：O(max(M,N))，M 和 N 分别是链表 a 和 b 的长度；
空间复杂度：O(1)，只使用了常数的空间。
