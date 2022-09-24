# 91. Decode Ways （欠 优化）

>A message containing letters from A-Z can be encoded into numbers using the following mapping:
```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```
>To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:
>- "AAJF" with the grouping (1 1 10 6)
>- "KJF" with the grouping (11 10 6)

>Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".
Given a string s containing only digits, **return the number of ways to decode it**.
The test cases are generated so that the answer fits in a 32-bit integer.
```
 Example 2:

Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

## Solution 1 (DP)
分析：这其实是一道字符串类的动态规划题，不难发现对于字符串 s 的某个位置 i 而言，我们只关心「位置 i 自己能否形成独立 item 」和「位置 i 能够与上一位置（i-1）能否形成 item」，而不关心 i-1 之前的位置。


>对于字符串 s 的任意位置 i 而言，**其存在三种情况**：
>- 只能由位置 i 的单独作为一个 item，设为 a，转移的前提是 a 的数值范围为 [1,9]，转移逻辑为 f[i]=f[i−1]。
>- 只能由位置 i 的与前一位置（i-1）共同作为一个 item，设为 b，转移的前提是 b 的数值范围为 [10,26]，转移逻辑为 f[i]=f[i−2]。
>- 位置 i 既能作为独立 item 也能与上一位置形成 item，转移逻辑为 f[i]=f[i−1]+f[i−2]。
```
因此，我们有如下转移方程：
f[i]=f[i−1],1⩽a≤9
f[i]=f[i−2],10⩽b⩽26
f[i]=f[i−1]+f[i−2],1⩽a≤9,10⩽b⩽26
​
```
其他细节：由于题目存在前导零，而前导零属于无效 item。可以进行特判，但个人习惯往字符串头部追加空格作为哨兵，追加空格既可以避免讨论前导零，也能使下标从 1 开始，简化 f[i-1] 等负数下标的判断。
```
// dp[i]: 为str[0..i-1]的译码方法总数
// 分情况讨论: (建立最优子结构)
//      (1)若s[i-1]='0',那么若s[i-2]= '1' or '2', 则dp[i] = dp[i-2]; 否则return 0
//          解释: s[i-2]+s[i-1]唯一被译码 ,不增加情况
//      (2)若s[i-2]='1', 则dp[i] = dp[i-1] + dp[i-2]
//          解释: s[i-1]与s[i]分开译码,为dp[i-1]; 合并译码,为dp[i-2]
//      (3)若s[i-2]='2' 且 '1'<=s[i-1]<='6' ,则dp[i] = dp[i-1] + dp[i-2]
//          解释: 同上
//      (4)否则，说明s[i-2]>'2' 或 s[i-2]='0', 则dp[i] = dp[i-1]
//          解释: s[i-2]与s[i-1]分开译码
```
```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        s = " " + s;
        char[] cs = s.toCharArray();
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int i = 1; i <= n; i++) { 
            // a : 代表「当前位置」单独形成 item
            // b : 代表「当前位置」与「前一位置」共同形成 item
            int a = cs[i] - '0', b = (cs[i - 1] - '0') * 10 + (cs[i] - '0');
            // 如果 a 属于有效值，那么 f[i] 可以由 f[i - 1] 转移过来
            if (1 <= a && a <= 9) f[i] = f[i - 1];
            // 如果 b 属于有效值，那么 f[i] 可以由 f[i - 2] 或者 f[i - 1] & f[i - 2] 转移过来
            if (10 <= b && b <= 26) f[i] += f[i - 2];
        }
        return f[n];
    }
}

```
>**时间复杂度**：共有 n 个状态需要被转移。复杂度为O(n)。
**空间复杂度**：O(n)。

> 接下来 继续优化
> 空间优化
不难发现，我们转移 f[i] 时只依赖 f[i-1] 和 f[i-2] 两个状态。
因此我们可以采用与「滚动数组」类似的思路，只创建长度为 3 的数组，通过取余的方式来复用不再需要的下标。 
(欠)
```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        s = " " + s;
        char[] cs = s.toCharArray();
        int[] f = new int[3];
        f[0] = 1;
        for (int i = 1; i <= n; i++) {
            f[i % 3] = 0;
            int a = cs[i] - '0', b = (cs[i - 1] - '0') * 10 + (cs[i] - '0');
            if (1 <= a && a <= 9) f[i % 3] = f[(i - 1) % 3];
            if (10 <= b && b <= 26) f[i % 3] += f[(i - 2) % 3];
        }
        return f[n % 3];
    }
}
```

