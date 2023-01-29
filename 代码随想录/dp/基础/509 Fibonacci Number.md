509. Fibonacci Number

The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.
Given n, calculate F(n).

 

Example 1:

Input: n = 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.

## S1
想象成滑动数组，顺序pqr， 把每次p+q的值传进r， 然后让原先的p消失，qr往前移到旧的pq的位置  产生新的r
```java
class Solution {
    //如果你也是跟我一样刚入门动态规划，那么我希望这道题你能用这个解法来理解动态规划
    //可以看官方的图。
    public int fib(int n) {
        //当小于2的时候，就之间返回
      if(n<2) return n;
      //对接题目
      //p的相当于数组的第一个位置，默认就是给0。这个可以先这样记住，理解不了的话
     //q相当于f(1)。而r相当于f(2)
      int p =0, q=0,r=1;
      for(int i =2;i<=n;i++){
          //将q的值赋给p，相当于p向后移动一格
          p = q;
          //将r的值赋值给q，相当于q向后移动一格。
          q = r;
          //r等于前两个的相加。n值多个这个就循环几次。
          r = p+q;
      }
      //每个结果都是前两个结果的和
      return r;
     }
}
```


```java
//非压缩状态的版本
class Solution {
    public int fib(int n) {
        if (n <= 1) return n;             
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int index = 2; index <= n; index++){
            dp[index] = dp[index - 1] + dp[index - 2];
        }
        return dp[n];
    }
}
```

时间复杂度：O(n)
空间复杂度：O(n)