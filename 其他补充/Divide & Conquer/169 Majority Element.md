# 169. Majority Element
(虽然我也不知道为什么是分治)
Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

 
```
Example 1:

Input: nums = [3,2,3]
Output: 3
```

## SOlution 1 (摩尔投票法)
```
摩尔投票法思路
候选人(cand_num)初始化为nums[0]，票数count初始化为1。
当遇到与cand_num相同的数，则票数count = count + 1，否则票数count = count - 1。
当票数count为0时，更换候选人，并将票数count重置为1。
遍历完数组后，cand_num即为最终答案。

为何这行得通呢？
投票法是遇到相同的则票数 + 1，遇到不同的则票数 - 1。
且“多数元素”的个数> ⌊ n/2 ⌋，其余元素的个数总和<= ⌊ n/2 ⌋。
因此“多数元素”的个数 - 其余元素的个数总和 的结果 肯定 >= 1。
这就相当于每个“多数元素”和其他元素 两两相互抵消，抵消到最后肯定还剩余至少1个“多数元素”。

无论数组是1 2 1 2 1，亦或是1 2 2 1 1，总能得到正确的候选人。

作者：gfu
链接：https://leetcode.cn/problems/majority-element/solution/3chong-fang-fa-by-gfu-2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
```java
class Solution {
    public int majorityElement(int[] nums) {
        int vote = nums[0];
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            // 票数count为0时，更换候选人，并将count重置为1
            if (count == 0) {
                vote = nums[i];
                count = 1;
                continue;
            }
            // 遇到相同的则票数 + 1，遇到不同的则票数 - 1
            if (nums[i] == vote) {
                count++;
            } else {
                count--;
            }
        }
        return vote;
    }
}
```