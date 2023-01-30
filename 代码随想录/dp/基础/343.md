# 343. Integer Break

Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers.

Return the maximum product you can get.

```
Example 1:

Input: n = 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
Example 2:

Input: n = 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```
## S1 (dp)
```
这题还可以这样想，对于给定的数 n，

F(n) 表示将 n 分解成多个(≥2)整数的最大乘积，那么有以下几种情况：
1⃣ 将n分解成两个数:

F(n)=i∗(n−i), (i=1,2,...,n−1)

2⃣ 将n分解成两个以上的数:
也就是说我们要对 i 和 n - i 进一步分解，那么就有三种情况（继续分解其中一个或两个都继续分解），将i和n - i进一步分解的最大乘积分别记为 

F(i)和

F(n−i)，那么有：

 继续分解

 都继续分解
F(n)=max 

  
i∗F(n−i)
F(i)∗(n−i)
F(i)∗F(n−i)
​
  
​
  
对 n−i 继续分解
对 i 继续分解
对 i和 n−i 都继续分解
​
 

但是，我们观察上面的表达式，不难发现，当i取遍

[1,n−1]时，由于i和n - i的取值对称，

i∗F(n−i) 和 

F(i)∗(n−i)的取值集合是一样的，也就是说这两种情况算出来的结果是一样的，所以只取其中一种就可以了。对于

F(i)∗F(n−i)，其实这种情况也是不用考虑的，因为 

i∗(n−i)和

i∗F(n−i)已经包含了所有的分解情况了。那么整个表达式就为：

F(n)=max{i∗(n−i),i∗F(n−i)}, (i=1,2,...,n−1)

作者：97wgl
链接：https://leetcode.cn/problems/integer-break/solution/bao-li-sou-suo-ji-yi-hua-sou-suo-dong-tai-gui-hua-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```
```java
class Solution {
    public int integerBreak(int n) {
        //dp[i] 为正整数 i 拆分后的结果的最大乘积
        int[] dp = new int[n+1];
        dp[2] = 1;
        for(int i = 3; i <= n; i++) {
            for(int j = 1; j <= i-j; j++) {
                // 这里的 j 其实最大值为 i-j,再大只不过是重复而已，
                //并且，在本题中，我们分析 dp[0], dp[1]都是无意义的，
                //j 最大到 i-j,就不会用到 dp[0]与dp[1]
                dp[i] = Math.max(dp[i], Math.max(j*(i-j), j*dp[i-j]));
                // j * (i - j) 是单纯的把整数 i 拆分为两个数 也就是 i,i-j ，再相乘
                //而j * dp[i - j]是将 i 拆分成两个以及两个以上的个数,再相乘。
            }
        }
        return dp[n];
    }
}
```
时间复杂度：O(n^2)
空间复杂度：O(n)


## S2 
数学解法： https://leetcode.cn/problems/integer-break/solution/343-zheng-shu-chai-fen-tan-xin-by-jyd/