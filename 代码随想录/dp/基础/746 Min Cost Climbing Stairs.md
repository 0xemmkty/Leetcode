# 746. Min Cost Climbing Stairs

You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index 0, or the step with index 1.

Return the minimum cost to reach the top of the floor.

 
```
Example 1:

Input: cost = [10,15,20]
Output: 15
Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.
```
其实大佬的上面分成两种情况，本质就是对于在 i 阶楼梯是否包含cost[i]情况的分类。若是不包含 i 楼梯，相当于数组中的左闭右开区间，则在经过 i 阶楼梯时需要额外加上 i 阶楼梯上的体力值。若是包含 i 楼梯，则在经过 i 楼梯时，不需要加上 i 楼梯，相当于数组中的左闭右闭区间。

https://leetcode.cn/problems/min-cost-climbing-stairs/solution/yi-bu-yi-bu-tui-dao-dong-tai-gui-hua-de-duo-chong-/
## S1
理解题意需要注意两点：

第i级台阶是第i-1级台阶的阶梯顶部。

踏上第i级台阶花费cost[i]，直接迈一大步跨过而不踏上去则不用花费

到达第i级台阶的阶梯顶部的最小花费，有两个选择：

先付出最小总花费minCost[i-1]到达第i级台阶（即第i-1级台阶的阶梯顶部），踏上第i级台阶需要再花费cost[i]，再迈一步到达第i级台阶的阶梯顶部，最小总花费为minCost[i-1] + cost[i])；

先付出最小总花费minCost[i-2]到达第i-1级台阶（即第i-2级台阶的阶梯顶部），踏上第i-1级台阶需要再花费cost[i-1]，再迈两步跨过第i级台阶直接到达第i级台阶的阶梯顶部，最小总花费为minCost[i-2] + cost[i-1])；

则minCost[i]是上面这两个最小总花费中的最小值。

minCost[i] = min(minCost[i-1] + cost[i], minCost[i-2] + cost[i-1])。

台阶的数组从0开始计数。可以用-1代表地面，并设cost[-1] = 0。

最小总花费的初始值为：

第0级台阶： minCost[0] = min(cost[-1], cost[0]) = min(0, cost[0]) = 0，

第1级台阶： minCost[1] = min(cost[0], cost[1])。

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int size = cost.length;
        int[] minCost = new int[size];
        minCost[0] = 0;
        minCost[1] = Math.min(cost[0], cost[1]);
        for (int i = 2; i < size; i++) {
            minCost[i] = Math.min(minCost[i - 1] + cost[i], minCost[i - 2] + cost[i - 1]);
        }
        return minCost[size - 1];
    }
}
```

优化
上面的代码在空间利用上可以再优化一下。只用两个变量保存状态转移方程中前面的两个记录，并不断更新，就可以递推下去，这样空间复杂度就由O(N)变为O(1)了
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int minCost0 = 0;
        int minCost1 = Math.min(cost[0], cost[1]);
        int minCost = 0;
        for (int i = 2; i < cost.length; i++) {
            minCost = Math.min(minCost1 + cost[i], minCost0 + cost[i - 1]);
            minCost0 = minCost1;
            minCost1 = minCost;
        }
        return minCost;
    }
};
```

## s2
```
因此状态转移方程是：

dp[i] = min(dp[i-2], dp[i-1]) + cost[i]。

初始条件：

最后一步踏上第0级台阶，最小花费dp[0] = cost[0]。

最后一步踏上第1级台阶有两个选择：

可以分别踏上第0级与第1级台阶，花费cost[0] + cost[1]；
也可以从地面开始迈两步直接踏上第1级台阶，花费cost[1]。
最小值dp[1] = min(cost[0] + cost[1], cost[1]) = cost[1]。

```
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length];
        dp[0] = cost[0];
        dp[1] = cost[1];
        for (int i = 2; i < cost.length; i++) {
            dp[i] = Math.min(dp[i - 2], dp[i - 1]) + cost[i];
        }
        return Math.min(dp[cost.length - 2], dp[cost.length - 1]);
    }
}
```
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        for (int i = 2; i < cost.length; i++) {
            cost[i] = Math.min(cost[i - 2], cost[i - 1]) + cost[i];
        }
        return Math.min(cost[cost.length - 2], cost[cost.length - 1]);
    }
}

```
