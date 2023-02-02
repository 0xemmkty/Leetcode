# 1049. Last Stone Weight II

You are given an array of integers stones where stones[i] is the weight of the ith stone.

We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights x and y with x <= y. The result of this smash is:

If x == y, both stones are destroyed, and
If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.
At the end of the game, there is at most one stone left.

Return the smallest possible weight of the left stone. If there are no stones left, return 0.

## S1 (dp)
01背包的递推公式为：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

本题则是：dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
一维数组
```java 
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int i : stones) {
            sum += i;
        }
        int target = sum >> 1;
        //初始化dp数组
        int[] dp = new int[target + 1];
        for (int i = 0; i < stones.length; i++) {
            //采用倒序
            for (int j = target; j >= stones[i]; j--) {
                //两种情况，要么放，要么不放
                dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }
        return sum - 2 * dp[target];
    }
}
```

二维数组
```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int s : stones) {
            sum += s;
        }

        int target = sum / 2;
        //初始化，dp[i][j]为可以放0-i物品，背包容量为j的情况下背包中的最大价值
        int[][] dp = new int[stones.length][target + 1];
        //dp[i][0]默认初始化为0
        //dp[0][j]取决于stones[0]
        for (int j = stones[0]; j <= target; j++) {
            dp[0][j] = stones[0];
        }

        for (int i = 1; i < stones.length; i++) {
            for (int j = 1; j <= target; j++) {//注意是等于
                if (j >= stones[i]) {
                    //不放:dp[i - 1][j] 放:dp[i - 1][j - stones[i]] + stones[i]
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - stones[i]] + stones[i]);
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        System.out.println(dp[stones.length - 1][target]);
        return (sum - dp[stones.length - 1][target]) - dp[stones.length - 1][target];
    }
}
```
 