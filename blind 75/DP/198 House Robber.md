# 198. House Robber

>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will **automatically contact the police if two adjacent houses were broken into on the same night**.

>Given an integer array nums representing the amount of money of each house, return the **maximum amount of money** you can rob tonight without alerting the police.

```
Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```
>reference link:
https://leetcode.cn/problems/house-robber/solution/dong-tai-gui-hua-jie-ti-si-bu-zou-xiang-jie-cjavap/
## Solution 1 (DP)

>动态规划的的四个解题步骤是：
>
>>定义子问题
写出子问题的递推关系
确定 DP 数组的计算顺序
空间优化（可选）
>
>**定义子问题**
例如这道小偷问题，原问题是“从全部房子中能偷到的最大金额”，将问题的规模缩小，子问题就是“从 k 个房子中能偷到的最大金额”，用 f(k) 表示。
>
>**写出子问题的递推关系**
f(k)=max{f(k−1),H_k−1 +f(k−2)}
(两种类型：1，最后一间不偷，偷前k-1间
        2， 偷k-2间和最后一间)
>
>**确定 DP 数组的计算顺序**
        对于小偷问题，我们分析子问题的依赖关系，发现每个 f(k)f(k) 依赖 f(k-1)f(k−1) 和 f(k-2)f(k−2)。也就是说，dp[k] 依赖 dp[k-1] 和 dp[k-2]，如下图所示。

代码 但仍需要优化
```java 

public int rob(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    // 子问题：
    // f(k) = 偷 [0..k) 房间中的最大金额

    // f(0) = 0
    // f(1) = nums[0]
    // f(k) = max{ rob(k-1), nums[k-1] + rob(k-2) }

    int N = nums.length;
    int[] dp = new int[N+1];
    dp[0] = 0;
    dp[1] = nums[0];
    for (int k = 2; k <= N; k++) {
        dp[k] = Math.max(dp[k-1], nums[k-1] + dp[k-2]);
    }
    return dp[N];
}

```
（优化前，使用一维数组，优化后只使用两个变量）
这样一来，空间复杂度也从 O(n) 降到了O(1)
```java
class Solution {
    public int rob(int[] nums) {
    int prev = 0;
    int curr = 0;

    // 每次循环，计算“偷到当前房子为止的最大金额”
    for (int i : nums) {
        // 循环开始时，curr 表示 dp[k-1]，prev 表示 dp[k-2]
        // dp[k] = max{ dp[k-1], dp[k-2] + i }
        int temp = Math.max(curr, prev + i);
        prev = curr;
        curr = temp;
        // 循环结束时，curr 表示 dp[k]，prev 表示 dp[k-1]
    }

    return curr;
}


}
```


