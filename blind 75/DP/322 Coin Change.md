# 322. Coin Change (欠)

>You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.
Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
You may assume that you have an infinite number of each kind of coin.

```
Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```
https://leetcode.cn/problems/coin-change/solution/javadi-gui-ji-yi-hua-sou-suo-dong-tai-gui-hua-by-s/
该网址列出了四种方法 不断优化 很有参考价值
动态规划：自底而上
记忆化搜索：自顶而下
## Solution 1 (DP)
```
例子2：假设


coins = [1, 2, 3], amount = 6


在上图中，可以看到：

 F(3)
=min(F(3−1),F(3−2),F(3−3))+1
=min(F(2),F(1),F(0))+1
=min(1,1,0)+1
=1
​

```
```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int max = amount + 1;
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, max);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.length; j++) {
                if (coins[j] <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
// idea is quite strightforward
// pay  attention to two loop
```
>时间复杂度：O(Sn)，其中 S 是金额，n 是面额数。我们一共需要计算 O(S) 个状态，S 为题目所给的总金额。对于每个状态，每次需要枚举 nn 个面额来转移状态，所以一共需要 O(Sn) 的时间复杂度。
空间复杂度：O(S)。数组 dp 需要开长度为总金额 S 的空间。

## Solution 2 (记忆化搜索) 欠
```java
class Solution {
    int[] memo;
    public int coinChange(int[] coins, int amount) {
        if(coins.length == 0){
            return -1;
        }
        memo = new int[amount];

        return findWay(coins,amount);
    }
    // memo[n] 表示钱币n可以被换取的最少的硬币数，不能换取就为-1
    // findWay函数的目的是为了找到 amount数量的零钱可以兑换的最少硬币数量，返回其值int
    public int findWay(int[] coins,int amount){
        if(amount < 0){
            return -1;
        }
        if(amount == 0){
            return 0;
        }
        // 记忆化的处理，memo[n]用赋予了值，就不用继续下面的循环
        // 直接的返回memo[n] 的最优值
        if(memo[amount-1] != 0){
            return memo[amount-1];
        }
        int min = Integer.MAX_VALUE;
        for(int i = 0;i < coins.length;i++){
            int res = findWay(coins,amount-coins[i]);
            if(res >= 0 && res < min){
                min = res + 1; // 加1，是为了加上得到res结果的那个步骤中，兑换的一个硬币
            }
        }
        memo[amount-1] = (min == Integer.MAX_VALUE ? -1 : min);
        return memo[amount-1];
    }
}
```