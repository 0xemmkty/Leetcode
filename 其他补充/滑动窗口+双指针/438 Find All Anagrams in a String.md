438. Find All Anagrams in a String
Medium
11.9K
324
Companies
Given two strings s and p, return an array of all the start indices of p's anagrams in s. You may return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.


见手写笔记6
```python
class Solution(object):
    def findAnagrams(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: List[int]
        """
        if len(p) > len(s):
            return []

        # Initialize the result list
        result = []

        # Create two dictionaries to compare characters in the sliding window
        p_count, s_count = {}, {}

        # Populate the p_count dictionary with the frequency of characters in p
        for char in p:
            if char in p_count:
                p_count[char] += 1
            else:
                p_count[char] = 1

    # Initialize the sliding window
        left = 0
        for right in range(len(s)):
        # 扩大窗口：加
        # Add the current character to the s_count dictionary
            if s[right] in s_count:
                s_count[s[right]] += 1
            else:
                s_count[s[right]] = 1

        # Check if the current window size exceeds the length of p
        # If so, remove the leftmost character from the window
        # 超过窗口--删
            if right - left + 1 > len(p):
                if s_count[s[left]] == 1:
                    del s_count[s[left]]
                else:
                    s_count[s[left]] -= 1
                left += 1

        # Compare the dictionaries and add to result if they match
        # 重点
            if p_count == s_count:
                result.append(left)

        return result
```


O(n)