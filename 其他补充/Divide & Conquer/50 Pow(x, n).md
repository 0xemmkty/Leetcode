# 50. Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (i.e., xn).

分治思想！大问题化为小问题？

## Solution 1 (分治角度 快速幂)
「快速幂算法」的本质是分治算法。举个例子，如果我们要计算 x^{64}x 
64
 ，我们可以按照：

x→x^2→x^4→x^8→x^16→x^32→x^64
 

的顺序，从 x 开始，每次直接把上一次的结果进行平方，计算 6 次就可以得到 x^64的值，而不需要对 x 乘 63 次 x。

再举一个例子，如果我们要计算 x^{77}x 
77
 ，我们可以按照：

x \to x^2 \to x^4 \to x^9 \to x^{19} \to x^{38} \to x^{77}
x→x 
2
 →x 
4
 →x 
9
 →x 
19
 →x 
38
 →x 
77

作者：LeetCode-Solution
链接：https://leetcode.cn/problems/powx-n/solution/powx-n-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0.0f) return 0.0d;
        long b = n;
        double res = 1.0;
        if(b < 0) {
            x = 1 / x;
            b = -b;
        }
        while(b > 0) {
            if((b & 1) == 1) res *= x;
            x *= x;
            b >>= 1;
        }
        return res;
    }
}


```

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }

    public double quickMul(double x, long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }
}

```
时间复杂度：O(logn)，即为递归的层数。

空间复杂度：O(logn)，即为递归的层数。这是由于递归的函数调用会使用栈空间。
