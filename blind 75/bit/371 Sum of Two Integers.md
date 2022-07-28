# 371. Sum of Two Integers

> Given two integers a and b, return the sum of the two integers without using the operators + and -.

## Solution 1 (bit calcu)
```java
    class Solution {
    public int getSum(int a, int b) {
        return (b == 0) ? a : getSum(a ^ b, (a & b) << 1);
    }
    }
    // << 1 means each num move left once
```


解析：
    a = 5 = 0101
b = 4 = 0100

a ^ b 如下：

0 1 0 1  
0 1 0 0  
\-----------  
0 0 0 1

a ^ b(异或) 得到了一个**无进位加法结果**，如果要得到 a + b 的最终值，我们还要找到进位的数，把这二者相加。在位运算中，我们可以使用**与操作**获得进位：
a = 5 = 0101
b = 4 = 0100

a & b 如下：

0 1 0 1
0 1 0 0
\----------
0 1 0 0

由计算结果可见，0100 并不是我们想要的进位，1 + 1 所获得的进位应该要放置在它的更高位，即左侧位上，因此我们还要把 0100 左移一位，才是我们所要的进位结果。

那么问题就容易了，总结一下：

>1. a + b 的问题拆分为 (a 和 b 的无进位结果) + (a 和 b 的进位结果)
>2. 无进位加法使用异或运算计算得出
>3. 进位结果使用与运算和移位运算计算得出
>4. 循环此过程，直到进位为 0

作者：jalan
链接：https://leetcode.cn/problems/sum-of-two-integers/solution/wei-yun-suan-xiang-jie-yi-ji-zai-python-zhong-xu-y/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。