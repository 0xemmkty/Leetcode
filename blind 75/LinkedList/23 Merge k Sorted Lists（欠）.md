# 23. Merge k Sorted Lists

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

## Solution 1 (分治, Divide and conquer)
看solution 1 第二个写法
复杂度分析

时间复杂度：考虑递归「向上回升」的过程——第一轮合并 
2k
​
  组链表，每一组的时间代价是 O(2n)；第二轮合并 
4k 组链表，每一组的时间代价是O(4n)......所以总的时间代价是 O(\sum_{i = 1}^{\infty} \frac{k}{2^i} \times 2^i n) = O(kn \times \log k)O(∑ 
i=1
∞
​
  
2 
i
 
k
​
 ×2 
i
 n)=O(kn×logk)，故渐进时间复杂度为 O(kn \times \log k)O(kn×logk)。
空间复杂度：递归会使用到O(logk) 空间代价的栈空间。


```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        return merge(lists, 0, lists.length - 1);
    }

    public ListNode merge(ListNode[] lists, int l, int r) {
        if (l == r) {
            return lists[l];
        }
        if (l > r) {
            return null;
        }
        int mid = (l + r) >> 1;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null || b == null) {
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);
        ListNode tail = head, aPtr = a, bPtr = b;
        while (aPtr != null && bPtr != null) {
            if (aPtr.val < bPtr.val) {
                tail.next = aPtr;
                aPtr = aPtr.next;
            } else {
                tail.next = bPtr;
                bPtr = bPtr.next;
            }
            tail = tail.next;
        }
        tail.next = (aPtr != null ? aPtr : bPtr);
        return head.next;
    }
}

```

同思想，不同写法

```java
class Solution {
   public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        return merge(lists, 0, lists.length - 1);
    }

    private ListNode merge(ListNode[] lists, int left, int right) {
        if (left == right) return lists[left];
        int mid = left + (right - left) / 2;
        ListNode l1 = merge(lists, left, mid);
        ListNode l2 = merge(lists, mid + 1, right);
        return mergeTwoLists(l1, l2);
    }

    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
}
// 分成两两链表合并（合并部分见lc21）， 注意是两两链表合并（也就是给链表分类），而不是给链表中的元素两两分类
// mid = left + (right - left) / 2
// 思路：1.lists边界判断
// 2. 设置递归触底条件。
// 3. mid
// 4. 两部分分别递归
// 5.设置两两链表合并算法
// 为什么可以用分治？因为你不知道有多少list，但不论多少，都可以变成两两合并，而两两合并的算法我们已经写过了（lc21）
```

## Solution 2 （优先队列） 没懂 先欠着 总有一天会懂
时间复杂度：O(n∗log(k))
空间复杂度为 O(k)
>这个方法和前两种方法的思路有所不同，我们需要维护当前每个链表没有被合并的元素的最前面一个，kk 个链表就最多有 kk 个满足这样条件的元素，每次在这些元素里面选取 \textit{val}val 属性最小的元素合并到答案中。在选取最小元素的时候，我们可以用优先队列来优化这个过程。


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
   public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        PriorityQueue<ListNode> queue = new PriorityQueue<>(lists.length, new Comparator<ListNode>() {
            @Override
            public int compare(ListNode o1, ListNode o2) {
                if (o1.val < o2.val) return -1;
                else if (o1.val == o2.val) return 0;
                else return 1;
            }
        });
        ListNode dummy = new ListNode(0);
        ListNode p = dummy;
        for (ListNode node : lists) {
            if (node != null) queue.add(node);
        }
        while (!queue.isEmpty()) {
            p.next = queue.poll();
            p = p.next;
            if (p.next != null) queue.add(p.next);
        }
        return dummy.next;
    }
}

```

```java
class Solution {
    class Status implements Comparable<Status> {
        int val;
        ListNode ptr;

        Status(int val, ListNode ptr) {
            this.val = val;
            this.ptr = ptr;
        }

        public int compareTo(Status status2) {
            return this.val - status2.val;
        }
    }

    PriorityQueue<Status> queue = new PriorityQueue<Status>();

    public ListNode mergeKLists(ListNode[] lists) {
        for (ListNode node: lists) {
            if (node != null) {
                queue.offer(new Status(node.val, node));
            }
        }
        ListNode head = new ListNode(0);
        ListNode tail = head;
        while (!queue.isEmpty()) {
            Status f = queue.poll();
            tail.next = f.ptr;
            tail = tail.next;
            if (f.ptr.next != null) {
                queue.offer(new Status(f.ptr.next.val, f.ptr.next));
            }
        }
        return head.next;
    }
}


```

## SOlution 3(顺序合并)
耗时久
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode ans = null;
        for (int i = 0; i < lists.length; ++i) {
            ans = mergeTwoLists(ans, lists[i]);
        }
        return ans;
    }

    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null || b == null) {
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);
        ListNode tail = head, aPtr = a, bPtr = b;
        while (aPtr != null && bPtr != null) {
            if (aPtr.val < bPtr.val) {
                tail.next = aPtr;
                aPtr = aPtr.next;
            } else {
                tail.next = bPtr;
                bPtr = bPtr.next;
            }
            tail = tail.next;
        }
        tail.next = (aPtr != null ? aPtr : bPtr);
        return head.next;
    }
}


```

## Solution 
遍历所有链表，把结点放入一个动态数组，对动态数组排序，然后再把动态数组转成链表
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.PriorityQueue;

class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }

    ListNode(Integer[] nums) {
        ListNode currNode = this;
        currNode.val = nums[0];
        for (int i = 1; i < nums.length; i++) {
            currNode.next = new ListNode(nums[i]);
            currNode = currNode.next;
        }
    }

    @Override
    public String toString() {
        ListNode currNode = this;
        StringBuilder s = new StringBuilder();
        while (currNode != null) {
            s.append(currNode.val);
            s.append(" -> ");
            currNode = currNode.next;
        }
        // 最后添加一个 NULL 标志表示添加到末尾了
        s.append("NULL");
        return s.toString();
    }
}


public class Solution {

    public ListNode mergeKLists(ListNode[] lists) {
        List<Integer> arr = new ArrayList<>();

        for (ListNode listNode : lists) {
            if (listNode != null) {
                ListNode curNode = listNode;
                while (curNode != null) {
                    arr.add(curNode.val);
                    curNode = curNode.next;
                }
            }
        }

        Collections.sort(arr);

        ListNode dummyNode = new ListNode(-1);
        ListNode curNode = dummyNode;
        int size = arr.size();
        for (int i = 0; i < size; i++) {
            curNode.next = new ListNode(arr.get(i));
            curNode = curNode.next;
        }
        return dummyNode.next;
    }
}

作者：liweiwei1419
链接：https://leetcode.cn/problems/merge-k-sorted-lists/solutions/9053/tan-xin-suan-fa-you-xian-dui-lie-fen-zhi-fa-python/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```