# 190. Reverse Bits

>**Reverse bits** of a given **32 bits** unsigned integer.
Note:
>Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above, the input represents the signed integer -3 and the output represents the signed integer -1073741825.

## Solution 1 (循环)

>这是最容易想到的方法了，每次把 res 左移，把 n 的二进制末尾数字，拼接到结果 res 的末尾。然后把 n右移。
**时间复杂度**：O(1)
**空间复杂度**：O(1)

```java
public class Solution {
    
    public int reverseBits(int n) {
        int res = 0;
        for (int i = 0; i < 32; ++i) {
            res = (res << 1) | (n & 1);
            n >>= 1;
        }
        return res;
    }
}
//  | 为位运算符:两个数字的二进制相对应位置全都为0，那么则为0，否则为1
// n & 1 can get the last digit inside n
// since after excute res<<1 the last num is 0(fill), and we know that x | 0 = x，so this is very good way to let the last digit of n be put in the last position of res
// n>>=1 constantly get the last digit of n
```

```  位运算可以进行进行int数的或运算。
  例：4|1=5
  4写作二进制数 1 0 0
  1写作二进制数 0 0 1
  做或运算得出  1 0 1   即5
```

## Solution 2 (分而治之)
>有另外一种不使用循环的做法，类似于归并排序。
其思想是分而治之，把数字分为两半，然后交换这两半的顺序；然后把前后两个半段都再分成两半，交换内部顺序……直至最后交换顺序的时候，交换的数字只有 1 位。

作者：fuxuemingzhu
链接：https://leetcode.cn/problems/reverse-bits/solution/fu-xue-ming-zhu-xun-huan-yu-fen-zhi-jie-hoakf/

(先放着 还没看)
```java
public class Solution {
    private static final int M8 = 0x00ff00ff;  // 0b00000000111111110000000011111111
    private static final int M4 = 0x0f0f0f0f;  // 0b00001111000011110000111100001111
    private static final int M2 = 0x33333333;  // 0b00110011001100110011001100110011
    private static final int M1 = 0x55555555;  // 0b01010101010101010101010101010101

    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        n = n >>> 16 | n << 16;
        n = n >>> 8 & M8 | (n & M8) << 8;
        n = n >>> 4 & M4 | (n & M4) << 4;
        n = n >>> 2 & M2 | (n & M2) << 2;
        n = n >>> 1 & M1 | (n & M1) << 1;
        return n;
    }
}
```