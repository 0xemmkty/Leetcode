# 76. Minimum Window Substring

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```
## Solution 1
```python
class Solution:
    def minWindow(self, s, t):
        n, m = len(s), len(t)

        # Create a hash table to store the count of characters in t
        hash = [0] * 128
        for ch in t:
            hash[ord(ch)] -= 1

        res = ""
        cnt = 0
        j = 0
        for i in range(n):
            hash[ord(s[i])] += 1
            # if in t, <0 at first, so after add will be <=0
            # if not in t, at first is 0, after add will be >0
            if hash[ord(s[i])] <= 0:
                # count for the number of element it cover
                cnt += 1
            # 算法的思路是从最左开始一路往右，找到全部cover的情况（cnt=m）
            # 所以会出现一开始的最左有不需要的东西
            # 所以开始缩小窗口
            while cnt == m and hash[ord(s[j])] > 0:
                hash[ord(s[j])] -= 1
                j += 1

            # check empty or find the min(if current window is smaller than res)
            if cnt == m:
                if res == "" or len(res) > i - j + 1:
                    res = s[j:i + 1]

        return res

# Example usage
solution = Solution()
s = "ADOBECODEBANC"
t = "ABC"
print(solution.minWindow(s, t))

```

```python
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        chars = [ord(c) for c in s]
        chart = [ord(c) for c in t]
        n, m = len(chars), len(chart)

        hash_map = [0] * 128
        for ch in chart:
            hash_map[ch] -= 1

        res = ""
        i = j = cnt = 0
        while i < n:
            hash_map[chars[i]] += 1
            if hash_map[chars[i]] <= 0:
                cnt += 1
            while cnt == m and hash_map[chars[j]] > 0:
                hash_map[chars[j]] -= 1
                j += 1
            if cnt == m:
                if not res or len(res) > i - j + 1:
                    res = s[j:i + 1]
            i += 1

        return res

            
        

```