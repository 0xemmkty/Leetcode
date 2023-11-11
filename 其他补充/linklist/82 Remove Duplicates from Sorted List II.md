82. Remove Duplicates from Sorted List II
Medium
8.4K
216
Companies
Given the head of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.

```python
class Solution(object):
    def deleteDuplicates(self, head):
        if not head or not head.next:
            return head
        dummy = ListNode(0)
        dummy.next = head
        pre = dummy
        cur = head
        while cur:
            # 跳过当前的重复节点，使得cur指向当前重复元素的最后一个位置
            while cur.next and cur.val == cur.next.val:
                cur = cur.next
            if pre.next == cur:
                 # pre和cur之间没有重复节点，pre后移
                pre = pre.next
            else:
                # pre->next指向cur的下一个位置（相当于跳过了当前的重复元素）
                # 但是pre不移动，仍然指向已经遍历的链表结尾
                pre.next = cur.next
            cur = cur.next
        return dummy.next

```