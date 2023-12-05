# 20. Valid Parentheses

>Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
An input string is valid if:
Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.



```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if len(s)%2==1:
            return False
        
        stack = []
        for char in s:
            if char == "(":
                stack.append(")")
            elif char == "{":
                stack.append("}")

            elif char == "[":
                stack.append("]")

            else:
                # 这里的not stack是指有反的括号没有正的括号（stack是空的）
              if not stack or stack.pop() != char:
                return False
                # 如果stack不是空的说明有没有被匹配的反括号就会return false
        return not stack

            
        

```
## Solution 1
这个写法看不是太懂，看下一个
```java
class Solution {
    private static final Map<Character,Character> map = new HashMap<Character,Character>(){{
        put('{','}'); put('[',']'); put('(',')'); put('?','?');
    }};
    public boolean isValid(String s) {
        if(s.length() > 0 && !map.containsKey(s.charAt(0))) return false;
        LinkedList<Character> stack = new LinkedList<Character>() {{ add('?'); }};
        for(Character c : s.toCharArray()){
            if(map.containsKey(c)) stack.addLast(c);
            else if(map.get(stack.removeLast()) != c) return false;
        }
        return stack.size() == 1;
    }
}

```

另一个版本
```java
class Solution {
   public boolean isValid(String s) {
        if(s.isEmpty())
            return true;
        Stack<Character> stack=new Stack<Character>();
        for(char c:s.toCharArray()){
            if(c=='(')
                stack.push(')');
            else if(c=='{')
                stack.push('}');
            else if(c=='[')
                stack.push(']');
            else if(stack.empty()||c!=stack.pop())
                return false;
        }
        if(stack.empty())
            return true;
        return false;
    }
}
// stack is first in later out
// so use "c!=stack.pop()" to determind

```

clear way
```java
class Solution {
    boolean isValid(String s) {
  if ((s.length() & 1) == 1) return false;
  else {
    Deque<Character> p = new ArrayDeque<>(s.length());
    for (int i = 0; i < s.length(); i++)
      switch (s.charAt(i)) {
        case '(': p.push(')'); break;
        case '{': p.push('}'); break;
        case '[': p.push(']'); break;
        case ')': case '}': case ']': if (p.isEmpty() || p.pop() != s.charAt(i)) return false;
      }
    return p.isEmpty();
  }
}
}
```