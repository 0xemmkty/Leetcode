# 213. House Robber II

>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. **All houses at this place are arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

>Given an integer array nums representing the amount of money of each house, **return the maximum amount of money** you can rob tonight without alerting the police.

## Solution 1
环状排列意味着第一个房子和最后一个房子中只能选择一个偷窃，因此可以把此环状排列房间问题约化为两个单排排列房间子问题：

在不偷窃第一个房子的情况下（即 nums[1:]），最大金额是 p_1；
在不偷窃最后一个房子的情况下（即 nums[:n−1]），最大金额是 p_2
​
综合偷窃最大金额： 为以上两种情况的较大值，即max(p1,p2) 。

>最终的转移方程： dp[n+1]=max(dp[n],dp[n−1]+num)
初始状态：
前 0 间房子的最大偷窃价值为 0 ，即 dp[0]=0 。
返回值：
返回 dp 列表最后一个元素值，即所有房间的最大偷窃价值。
简化空间复杂度：
我们发现 dp[n] 只与 dp[n−1] 和 dp[n−2] 有关系，因此我们可以设两个变量 cur和 pre 交替记录，将空间复杂度降到 O(1) 。
复杂度分析：
**时间复杂度** O(N) ： 两次遍历 nums 需要线性时间；
**空间复杂度** O(1) ： cur和 pre 使用常数大小的额外空间。

```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 0) return 0;
        if(nums.length == 1) return nums[0];
        return Math.max(myRob(Arrays.copyOfRange(nums, 0, nums.length - 1)), 
                        myRob(Arrays.copyOfRange(nums, 1, nums.length)));
    }
    private int myRob(int[] nums) {
        int pre = 0, cur = 0, tmp;
        for(int num : nums) {
            tmp = cur;
            cur = Math.max(pre + num, cur);
            pre = tmp;
        }
        return cur;
    }
}
// interesting!!
// compared with lc198, it add two case
// outside of that two cases, all the idea is the same
// the way it divide into two case is really interesting!! 
// pat attention to the boundary (0, nums.length - 1) and (1, nums.length)
```