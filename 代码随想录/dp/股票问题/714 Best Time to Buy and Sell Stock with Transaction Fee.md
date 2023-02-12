# 714. Best Time to Buy and Sell Stock with Transaction Fee

>You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee.
Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 
```
Example 1:

Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

## Solution 1
1. 定义状态转移方程
定义二维数组 dp[n][2]：
dp[i][0] 表示第 i 天不持有可获得的最大利润；

dp[i][1] 表示第 i 天持有可获得的最大利润（注意是第 i 天持有，而不是第 i 天买入）。
定义状态转移方程：

不持有：
dp[i][0]=max(dp[i−1][0],dp[i−1][1]+prices[i]−fee)

对于今天不持有，可以从两个状态转移过来：1. 昨天也不持有；2. 昨天持有，今天卖出。两者取较大值。

持有：

dp[i][1]=max(dp[i−1][1],dp[i−1][0]−prices[i])

对于今天持有，可以从两个状态转移过来：1. 昨天也持有；2. 昨天不持有，今天买入。两者取较大值。

2. 给定转移方程初始值
对于第 0 天：

不持有： dp[0][0]=0
持有（即花了 price[0] 的钱买入）： dp[0][1]=−prices[0]
3. 写代码递推实现转移方程

二维：
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee); 
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[n - 1][0];
    }
}

```
空间优化：转移的时候，
dp[i] 只会从 dp[i−1] 转移得来，因此第一维可以去掉：

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int[] dp = new int[2];
        dp[0] = 0;
        dp[1] = -prices[0];
        for (int i = 1; i < n; i++) {
            int tmp = dp[0];
            dp[0] = Math.max(dp[0], dp[1] + prices[i] - fee); 
            dp[1] = Math.max(dp[1], tmp - prices[i]);
        }
        return dp[0];
    }
}
```
time complexity: O(n)
空间复杂度：O(n)，空间优化后是 O(1)。
