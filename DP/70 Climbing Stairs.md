# 70. Climbing Stairs

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

## Solution 1 (DP)
>本问题其实常规解法可以分成多个子问题，爬第n阶楼梯的方法数量，等于 2 部分之和
爬上 n−1 阶楼梯的方法数量。因为再爬1阶就能到第n阶
爬上 n−2 阶楼梯的方法数量，因为再爬2阶就能到第n阶
所以我们得到公式**dp[n]=dp[n−1]+dp[n−2]**
同时需要**初始化 dp[0]=1 和 dp[1]=1**
时间复杂度：**O(n)**
思路挺好理解的
```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }

}
```
```
 public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for(int i = 1; i <= target; i++){
            for(int j = 0; j < nums.length; j++){
                if(i >= nums[j]){
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
```

## Solution 2 (斐波那契数列变形)
当拓展思路用的
```java
class Solution {
    public int climbStairs(int n) {
        double sqrt_5 = Math.sqrt(5);
        double fib_n = Math.pow((1 + sqrt_5) / 2, n + 1) - Math.pow((1 - sqrt_5) / 2,n + 1);
        return (int)(fib_n / sqrt_5);
    }
}


```
时间复杂度：O(logn)
