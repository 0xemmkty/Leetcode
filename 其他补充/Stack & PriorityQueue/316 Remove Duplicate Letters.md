316. Remove Duplicate Letters
(the same as 1081)
Medium
8.3K
583
Companies
Given a string s, remove duplicate letters so that every letter appears once and only once. You must make sure your result is 
the smallest in lexicographical order
 among all possible results.

Example 1:

Input: s = "bcabc"
Output: "abc"

```python
class Solution(object):
    def removeDuplicateLetters(self, s):
        """
        :type s: str
        :rtype: str
        """
        stack = []
        # s = "cbacdcbc" -> {'c': 4, 'b': 2, 'a': 1, 'd': 1}.
        remain_counter = collections.Counter(s)

        for c in s:
            if c not in stack:
                # 确保字典序，如果新放入的字典序大于最里面（第一个pop）的且这个新放入的后面还有（count>0）
                while stack and c < stack[-1] and  remain_counter[stack[-1]] > 0:
                    stack.pop()
                stack.append(c)
            remain_counter[c] -= 1
        return ''.join(stack)
    
```
```
Let's go through an example to see how the removeDuplicateLetters method works. Consider the input string s = "cbacdcbc".

Counter Creation: count = Counter(s) creates a Counter object that counts the occurrences of each character in s. So, count would be {'c': 4, 'b': 2, 'a': 1, 'd': 1}.

Initialize Stack: stack = deque() starts with an empty deque (double-ended queue).

First Iteration (c):

count['c'] becomes 3.
Since stack is empty, c is appended to stack.
Second Iteration (b):

count['b'] becomes 1.
b is not in stack, so it's appended.
Third Iteration (a):

count['a'] becomes 0.
a is not in stack. The while loop compares a with the top of the stack (b), which is greater than a, and count['b'] is not 0. So, b is popped.
a is appended to stack.
Fourth Iteration (c):

count['c'] becomes 2.
c is in stack, so this iteration is skipped.
Fifth Iteration (d):

count['d'] becomes 0.
d is not in stack, so it's appended.
Sixth Iteration (c):

count['c'] becomes 1.
c is in stack, so this iteration is skipped.
Seventh Iteration (b):

count['b'] becomes 0.
b is not in stack. The while loop compares b with the top of the stack (d), which is greater than b, but count['d'] is 0, so the loop breaks.
b is appended to stack.
Eighth Iteration (c):

count['c'] becomes 0.
c is in stack, so this iteration is skipped.
Return Result: Finally, stack contains ['a', 'c', 'd', 'b']. The method returns "acdb".

So, for the input string "cbacdcbc", the method removeDuplicateLetters would return "acdb", which is the lexicographically smallest subsequence of distinct characters.
```