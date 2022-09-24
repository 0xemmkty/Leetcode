# 62. Unique Paths (学习优化 排列组合)

>There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

>Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

>The test cases are generated so that the answer will be less than or equal to 2 * 109.

## Solution 1 (排列组合)
```java
class Solution {
    public int uniquePaths(int m, int n) {
        long res = 1;
        for(int x = n, y = 1; y < m; x++, y++) {
            res = res * x / y; //不能写成 res *= x / y
        }
        return (int)res;
    }
}
```

## Solution 2 (DP)
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for(int i = 0; i < n; i++) {
            dp[0][i] = 1;
        }
        for(int j = 0; j < m; j++) {
            dp[j][0] = 1;
        }
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
// 看看代码就懂了吧...很简单的

```

优化1 空间复杂度 O(2n)
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] pre = new int[n];
        int[] cur = new int[n];
        Arrays.fill(pre, 1);
        Arrays.fill(cur,1);
        for (int i = 1; i < m;i++){
            for (int j = 1; j < n; j++){
                cur[j] = cur[j-1] + pre[j];
            }
            pre = cur.clone();
        }
        return pre[n-1]; 
    }
}

```

优化2：空间复杂度 O(n)
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] cur = new int[n];
        Arrays.fill(cur,1);
        for (int i = 1; i < m;i++){
            for (int j = 1; j < n; j++){
                cur[j] += cur[j-1] ;
            }
        }
        return cur[n-1];
    }
}

```