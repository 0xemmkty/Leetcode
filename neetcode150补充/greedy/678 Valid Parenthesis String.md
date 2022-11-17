# 678. Valid Parenthesis String

>Given a string s containing only three types of characters: '(', ')' and '*', return true if s is valid.

>The following rules define a valid string:

>Any left parenthesis '(' must have a corresponding right parenthesis ')'.
Any right parenthesis ')' must have a corresponding left parenthesis '('.
Left parenthesis '(' must go before the corresponding right parenthesis ')'.
'*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string "".

## Solution 1 模拟法
```java
class Solution {
    public boolean checkValidString(String s) {
        int cmin = 0, cmax = 0; // open parentheses count in range [cmin, cmax]
        for (char c : s.toCharArray()) {
            if (c == '(') {
                cmax++;
                cmin++;
            } else if (c == ')') {
                cmax--;
                cmin--;
            } else if (c == '*') {
                cmax++; // if `*` become `(` then openCount++
                cmin--; // if `*` become `)` then openCount--
                // if `*` become `` then nothing happens
                // So openCount will be in new range [cmin-1, cmax+1]
            }
            if (cmax < 0) return false; // Currently, don't have enough open parentheses to match close parentheses-> Invalid
                                        // For example: ())(
            cmin = Math.max(cmin, 0);   // It's invalid if open parentheses count < 0 that's why cmin can't be negative
        }
        return cmin == 0; // Return true if can found `openCount == 0` in range [cmin, cmax]
    }
}
```
other version
```java
class Solution {
    public boolean checkValidString(String s) {
        // l: 左括号最少可能有多少个
        // r: 左括号最多可能有多少个
        int l = 0, r = 0;
        for (char c : s.toCharArray()) {
            // 遇到'('所有可能性加一
            // 遇到')'所有可能性减一
            // 遇到'*'，最少的可能性可以变少，最多的可能性也同样可以变多，这取决于这个星号最终我们看成什么，但是可能性都在
            if (c == '(') {
                l++; r++;
            } else if (c == ')') {
                l--; r--;
            } else {
                l--; r++;
            }
            // 当前左括号最少个数不能为负
            l = Math.max(l, 0);
            // 这种情况其实发生在r本身是负数的时候，也就是我们常见的右括号太多了
            if (l > r) return false;
        }
        // 能取到0个左括号才是满足平衡的
        return l == 0;
    }
}
```
时间复杂度：O(n)
空间复杂度：O(1)

## solution 2 (dp)
优美！
动态规划

定义 f[i][j] 为考虑前 i 个字符（字符下标从 1 开始），能否与 j 个右括号形成合法括号序列。

起始时只有 f[0][0] 为 true，最终答案为 f[n][0]。

不失一般性的考虑 f[i][j] 该如何转移：

当前字符为 ( : 如果 f[i][j] 为 true，必然有 f[i−1][j−1] 为 true，反之亦然。即有 f[i][j]=f[i−1][j−1]；(自：因为该括号是左括号，所以不影响右括号，所以退回到上一个和当前是一样的值)
当前字符为 ) : 如果 f[i][j] 为 true，必然有 f[i−1][j+1] 为 true，反之亦然。即有 f[i][j]=f[i−1][j+1]；（自：因为此处是右括号，所以退回到上一个时要让右括号+1，也就是j+1）
当前字符为 * : 根据 * 代指的符号不同，分为三种情况，只有有一种情况为 true 即可。即有 f[i][j]=f[i−1][j−1]∨f[i−1][j+1]∨f[i−1][j]。
```java
class Solution {
    public boolean checkValidString(String s) {
        int n = s.length();
        boolean[][] f = new boolean[n + 1][n + 1];
        f[0][0] = true;
        for (int i = 1; i <= n; i++) {
            char c = s.charAt(i - 1);
            for (int j = 0; j <= i; j++) {
                if (c == '(') {
                    if (j - 1 >= 0) f[i][j] = f[i - 1][j - 1];
                } else if (c == ')') {
                    if (j + 1 <= i) f[i][j] = f[i - 1][j + 1];
                } else {
                    f[i][j] = f[i - 1][j];
                    if (j - 1 >= 0) f[i][j] |= f[i - 1][j - 1];
                    if (j + 1 <= i) f[i][j] |= f[i - 1][j + 1];
                }
            }
        }
        return f[n][0];
    }
}
```
时间复杂度：O(n^2)
空间复杂度：O(n^2)
