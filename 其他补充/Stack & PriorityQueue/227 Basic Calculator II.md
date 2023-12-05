227. Basic Calculator II
```
Medium
5.9K
775
Companies
Given a string s which represents an expression, evaluate this expression and return its value. 

The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of [-231, 231 - 1].

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().
```

```python
class Solution(object):
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        sign="+"
        num=0
        stack=[]
        # 两个if都要check ‘ ’
        for i in range(len(s)):
            if s[i] != ' ' and s[i].isdigit():
                num = num*10+int(s[i])
            # 注意这里i=len(s)-1,如果没有这步最后一个数字就不会计算
            if (not s[i].isdigit() and s[i] != ' ') or i == len(s)-1:
                if sign == "+":
                    stack.append(num)
                elif sign == "-":
                    stack.append(-num)
                elif sign == "*":
                    stack.append(stack.pop()*num)
                elif sign =="/":
                    temp = stack.pop()
                    # 这里不确定
                    if temp // num < 0 and temp % num != 0:
                        stack.append(temp // num + 1)  # Adjust for Python's division towards negative infinity
                    else:
                        stack.append(temp // num)
                # update sign
                sign = s[i]
                num = 0
        return sum(stack)

```