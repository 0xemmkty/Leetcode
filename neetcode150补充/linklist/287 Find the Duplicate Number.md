# 287. Find the Duplicate Number

>Given an array of integers nums containing n + 1 integers where each integer is in the **range [1, n] **inclusive.

>There is **only one repeated number in nums**, return this repeated number.

>You must solve the problem without modifying the array nums and uses only constant extra space.

## Solution 1 (linklistin cycle, fast and slow pointer)
https://leetcode.cn/problems/find-the-duplicate-number/solution/287xun-zhao-zhong-fu-shu-by-kirsche/
```
首先明确前提，整数的数组 nums 中的数字范围是 [1,n]。考虑一下两种情况：

如果数组中没有重复的数，以数组 [1,3,4,2]为例，我们将数组下标 n 和数 nums[n] 建立一个映射关系f(n)，
其映射关系 n->f(n)为：
0->1
1->3
2->4
3->2
我们从下标为 0 出发，根据 f(n) 计算出一个值，以这个值为新的下标，再用这个函数计算，以此类推，直到下标超界。这样可以产生一个类似链表一样的序列。
0->1->3->2->4->null
如果数组中有重复的数，以数组 [1,3,4,2,2] 为例,我们将数组下标 n 和数 nums[n] 建立一个映射关系 f(n)，
其映射关系 n->f(n) 为：
0->1
1->3
2->4
3->2
4->2
同样的，我们从下标为 0 出发，根据 
f(n) 计算出一个值，以这个值为新的下标，再用这个函数计算，以此类推产生一个类似链表一样的序列。
0->1->3->2->4->2->4->2->……
至此，问题转换为 142 题。那么针对此题，快、慢指针该如何走呢。根据上述数组转链表的映射关系，可推出
142 题中慢指针走一步 slow = slow.next ==> 本题 slow = nums[slow]
142 题中快指针走两步 fast = fast.next.next ==> 本题 fast = nums[nums[fast]]

e4

```
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = 0;
        int fast = 0;
        slow = nums[slow]; // move forard one steo
        fast = nums[nums[fast]]; // move forward two step
        // slow!=fast, continue
        while(slow != fast){
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        // 我不好说这一步怎么回事
        // 只能说pre1从0开始一步一步建立“链表”联系
        // pre2更像是在“链表”的环那里一直循环
        // 相等的时候就是答案
        int pre1 = 0;
        int pre2 = slow;
        while(pre1 != pre2){
            pre1 = nums[pre1];
            pre2 = nums[pre2];
        }
        return pre1;
    }
}
```

链表中的环

假设有这样一个样例：[1,2,3,4,5,6,7,8,9,5]。如果我们按照上面的循环下去就会得到这样一个路径: 1 2 3 4 5 [6 7 8 9] [6 7 8 9] [6 7 8 9] . . .这样就有了一个环，也就是6 7 8 9。point 会一直在环中循环的前进。
这时我们设置两个一快(fast)一慢(slow)两个指针，一个每次走两步，一个每次走一步，这样让他们一直走下去，直到他们在重复的序列中相遇，

如上图，slow和fast会在环中相遇，先假设一些量：起点到环的入口长度为m，环的周长为c，在fast和slow相遇时slow走了n步。则fast走了2n步，fast比slow多走了n步，而这n步全用在了在环里循环（n%c==0）。
当fast和last相遇之后，我们设置第三个指针finder，它从起点开始和slow(在fast和slow相遇处)同步前进，当finder和slow相遇时，就是在环的入口处相遇，也就是重复的那个数字相遇。

为什么 finder 和 slow 相遇在入口

fast 和 slow 相遇时，slow 在环中行进的距离是n-m，其中 n%c==0。这时我们再让 slow 前进 m 步——也就是在环中走了 n 步了。而 n%c==0 即 slow 在环里面走的距离是环的周长的整数倍，就回到了环的入口了，而入口就是重复的数字。
我们不知道起点到入口的长度m，所以弄个 finder 和 slow 一起走，他们必定会在入口处相遇。

作者：zjczxz
链接：https://leetcode.cn/problems/find-the-duplicate-number/solution/kuai-man-zhi-zhen-de-jie-shi-cong-damien_undoxie-d/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## Solution 2 (原地哈希)
（但好像不符合题意因为修改了数组）

数组长度为 n+1，同时给定的 nums[i] 都在 [1,n] 范围内，因此我们可以设定哈希规则为 nums[idx]=idx+1，即数值 x 会放在下标 
x−1 的位置。

如此一来，对于只出现一次的数值而言，必然能够顺利放在目标位置，而出现多次的数值必然会因为位置冲突而被找出。

作者：AC_OIer
链接：https://leetcode.cn/problems/find-the-duplicate-number/solution/by-ac_oier-az8v/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```java
class Solution {
    public int findDuplicate(int[] nums) {
        for (int i = 0; i < nums.length; ) {
            int t = nums[i], idx = t - 1;
            if (nums[idx] == t) {
                if (idx != i) return t;
                i++;
            } else {
                swap(nums, idx, i);
            }
        }
        return -1;
    }
    void swap(int[] nums, int i, int j) {
        int c = nums[i];
        nums[i] = nums[j];
        nums[j] = c;
    }
}
时间复杂度：
O(n)
空间复杂度：
O(1)

```

## solution 3（二分查找）
```java
class Solution {

    public int findDuplicate(int[] nums) {
        int len = nums.length; // n + 1 = len, n = len - 1

        // 在 [1..n] 查找 nums 中重复的元素
        int left = 1;
        int right = len - 1;
        while (left < right) {
            int mid = (left + right) / 2;

            // nums 中小于等于 mid 的元素的个数
            int count = 0;
            for (int num : nums) {
                if (num <= mid) {
                    count++;
                }
            }

            if (count > mid) {
                // 下一轮搜索的区间 [left..mid]
                right = mid;
            } else {
                // 下一轮搜索的区间 [mid + 1..right]
                left = mid + 1;
            }
        }
        return left;
    }
}
```
复杂度分析：

时间复杂度：
O(NlogN)，二分法的时间复杂度为 
O(logN)，在二分法的内部，执行了一次 for 循环，时间复杂度为 O(N)，故时间复杂度为 O(NlogN)。
空间复杂度：O(1)，使用了一个 cnt 变量，因此空间复杂度为 O(1)。

```
9种方法。

这些方法可以分为以下几类：

需要额外空间，需要修改原始数组 
O
(
l
o
g
n
)
O(logn) space
排序 - 
O
(
n
l
o
g
n
)
O(nlogn) time
需要额外空间，不修改原始数组 
O
(
n
)
O(n) space
计数法 - 
O
(
n
)
O(n) time
Set - 
O
(
n
)
O(n) time
不需要额外空间，需要修改原始数组 
O
(
1
)
O(1) space
标记法 - 
O
(
n
)
O(n) time
索引排序 - 
O
(
n
)
O(n) time
不需要额外空间，不修改原始数组 
O
(
1
)
O(1) space
暴力 - 
O
(
n
2
)
O(n 
2
 ) time
二分搜索 - 
O
(
n
l
o
g
n
)
O(nlogn) time
位运算 - O(nlogn) time
双指针 - 
O
(
n
)
O(n) time


作者：longluo
链接：https://leetcode.cn/problems/find-the-duplicate-number/solution/by-longluo-e315/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
见9种方法归纳
https://leetcode.cn/problems/find-the-duplicate-number/solution/by-longluo-e315/