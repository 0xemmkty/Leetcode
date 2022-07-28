#338. Counting Bits

>Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.

## Solution 1
**分析过程很重要** 很聪明的解题！！

奇数：二进制表示中，奇数一定比前面那个偶数多一个 1，因为多的就是最低位的 1。
```
          举例： 
         0 = 0       1 = 1
         2 = 10      3 = 11
```
偶数：二进制表示中，偶数中 1 的个数一定和除以 2 之后的那个数一样多。因为最低位是 0，除以 2 就是右移一位，也就是把那个 0 抹掉而已，所以 1 的个数是不变的。
```
           举例：
          2 = 10       4 = 100       8 = 1000
          3 = 11       6 = 110       12 = 1100
```
作者：duadua
链接：https://leetcode.cn/problems/counting-bits/solution/hen-qing-xi-de-si-lu-by-duadua/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```java
class Solution {
      public int[] countBits(int num) {
        int[] result = new int[num + 1];
        for(int i = 1; i <= num; i++)
        {
            if(i % 2 == 1)
            {
                result[i] = result[i-1] + 1;
            }
            else
            {
                result[i] = result[i/2];
            }
        }
        return result;
    }
}
// new int[num+1]
//two case
```