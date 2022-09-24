# 300. Longest Increasing Subsequence （欠）

>Given an integer array nums, return the length of the longest strictly increasing subsequence.

>A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].

https://leetcode.cn/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-2/
## Solution 1 (DP + Dichotomy) (欠)
3ms
解题思路：
降低复杂度切入点： 解法一中，遍历计算 dp 列表需 O(N)，计算每个 dp[k] 需 O(N)。
动态规划中，通过线性遍历来计算 dp 的复杂度无法降低；
每轮计算中，需要通过线性遍历 [0,k) 区间元素来得到 dp[k] 。我们考虑：是否可以通过重新设计状态定义，使整个 dpdp 为一个排序列表；这样在计算每个 dp[k] 时，就可以通过二分法遍历 [0,k) 区间元素，将此部分复杂度由 O(N) 降至 O(logN)

复杂度分析：
时间复杂度 O(NlogN) ： 遍历 nums 列表需 O(N)，在每个 nums[i] 二分法需 O(logN)。
空间复杂度 O(N) ： tails 列表占用线性大小额外空间。



```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int res = 0;
        for(int num : nums) {
            int i = 0, j = res;
            while(i < j) {
                int m = (i + j) / 2;
                if(tails[m] < num) i = m + 1;
                else j = m;
            }
            tails[i] = num;
            if(res == j) res++;
        }
        return res;
    }
}
```
## Solution 2 (DP)
66ms
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0) return 0;
        int[] dp = new int[nums.length];
        int res = 0;
        Arrays.fill(dp, 1);
        for(int i = 0; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
// idea is strightforward
// j is infront of i
// so, if i>j, i can always be the back of j which means dp[j]+1
// so, what we need to do is Math.max(dp[i], dp[j] + 1)
```
>**时间复杂度 O(N^2)** ： 遍历计算 dp 列表需 O(N)，计算每个 dp[i] 需 O(N)。
**空间复杂度 O(N)** ： dp 列表占用线性大小额外空间。



