5. Longest Palindromic Substring
Medium
27.9K
1.7K
Companies
Given a string s, return the longest 
palindromic
 
substring
 in s.

 

Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.

expand the center
```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        if not s:
            return ""
        
        start = 0
        end = 0
        for i in range(len(s)):
            len1 = self._expandAroundCenter(s, i, i)         # treat character itself as a center
            len2 = self._expandAroundCenter(s, i, i + 1)    # treat space between characters as a center
            max_len = max(len1, len2)
            
            if max_len > end - start:
                start = i - (max_len - 1) // 2
                end = i + max_len // 2

        return s[start:end+1]

    def _expandAroundCenter(self, s, left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
```