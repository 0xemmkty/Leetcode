## 2262. Total Appeal of A String

The appeal of a string is the number of distinct characters found in the string.

For example, the appeal of "abbca" is 3 because it has 3 distinct characters: 'a', 'b', and 'c'.
Given a string s, return the total appeal of all of its substrings.

A substring is a contiguous sequence of characters within a string.

## Solution 1 哈希表+dp

dp记录每个下标元素作为子序列尾部的总引力
哈希表记录每个元素出现的最大下标
当前下标为i,上一个元素下标为j时, 子序列起始下标在[0, j]之间时引力不变, [j + 1, i]之间时引力 + 1
所以dp的转移方程 dp[i + 1] = dp[i] + i - j
如此即可求解.

python
```python
class Solution:
    def appealSum(self, s: str) -> int:
        l = len(s)
        dp = [0 for i in range(l + 1)]
        d = {}
        for i, n in enumerate(s):
            dp[i + 1] = dp[i] + i - d.get(n, -1) #当不存在j时,j为-1
            d[n] = i
        return sum(dp)
```