# 29. Divide Two Integers

>Given two integers dividend and divisor, divide two integers without using multiplication, division, and mod operator.

>The integer division should truncate toward zero, which means losing its fractional part. For example, 8.345 would be truncated to 8, and -2.7335 would be truncated to -2.

>Return the quotient after dividing dividend by divisor.

>Note: Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. For this problem, if the quotient is strictly greater than 231 - 1, then return 231 - 1, and if the quotient is strictly less than -231, then return -231.

## Solution 1(二分+倍增)

二分 + 倍增乘法（有意思）

由于题目限定了我们不能使用乘法、除法和 mod 运算符。

>我们可以先实现一个「倍增乘法」，然后利用对于 x 除以 y，结果 x / y 必然落在范围 **[0, x]** 的规律进行二分。


```java
    /**
     * 两数相除
     *
     * @param dividend 被除数
     * @param divisor 除数
     * @return 商（不包含小数）
     */
    public static int divide(int dividend, int divisor) {
        long result = 0;
        long x = dividend;
        long y = divisor;
        boolean negative = (x < 0 && y > 0) || (x > 0 && y < 0);

        if (x < 0) {
            x = -x;
        }
        if (y < 0) {
            y = -y;
        }
        // 由于x/y的结果肯定在[0,x]范围内，所以对x使用二分法
        long left = 0;
        long right = x;
        while (left < right) {
            long mid = left + right + 1 >> 1;
            if (quickMulti(mid, y) <= x) {
                // 相乘结果不大于x，左指针右移
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        result = negative ? -left : left;

        // 判断是否溢出
        if (result < Integer.MIN_VALUE || result > Integer.MAX_VALUE) {
            return Integer.MAX_VALUE;
        }
        
        return (int)result;
    }

    /**
     * 快速乘法
     *
     * @param a 乘数
     * @param b 被乘数
     * @return 积
     */
    public static long quickMulti(long a, long b) {
        long result = 0;

        while (b > 0) {
            if ((b & 1) == 1) {
                // 当前最低位为1，结果里加上a
                result += a;
            }
            // 被乘数右移1位，相当于除以2
            b >>= 1;
            // 乘数倍增，相当于乘以2
            a += a;
        }

        return result;
    }

// 倍增非常漂亮！！
// 怎么算两数相乘的结果呢？
// 把一个数“转移”到另一个数
// 跳出循环条件（if处）：b为1（这说明b已经完全转移到a，这是a就是答案）
// 超级喜欢这个算法，真机智hhh
```