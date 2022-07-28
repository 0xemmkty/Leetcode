# 191. Number of 1 Bits

>Write a function that takes an unsigned integer and **returns the number of '1' bits it has** (also known as the **Hamming weight**).
Note:
Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 3, the input represents the signed integer. -3.
 
## Soluton 1
时间复杂度：O(k)，k 为 n 的二进制长度。
空间复杂度：O(1)。

 ```java
 public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        for (int i = 0; i < 32; ++i) {
            count += n & 1;
            n >>= 1;
        }
        return count;
    }
}
// 1. 32
// 2. n & 1 = 1(n's last bit is 1) or 0
// 3. move right (let front 1 be the last 1)
```

## Solution 2

时间复杂度：O(k)，k 为 n 的二进制长度。
空间复杂度：O(1)。

>有个更为神奇的做法，那就是 n & (n - 1) ，这个代码可以把 n 的二进制中，最后一个出现的 1 改写成 0

n: 11  1(first 1)  0000  
n-1:11(same) 0   1111  
n&(n-1): 11   0   00000  

>因此执行 n & (n - 1) 使得 n 变成 0 的操作次数，就是 n 的二进制中 1 的个数。

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int res = 0;
        while (n != 0) {
            res += 1;
            n &= n - 1;
        }
        return res;
    }
}
//n & (n-1) can elinimate the first 1(from right to left)
// therefore we should compare the 0 to the n, if n!= 0, there still a 1 inside n, res can +1
```