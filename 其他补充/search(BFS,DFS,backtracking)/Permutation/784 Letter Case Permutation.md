# 784. Letter Case Permutation

Given a string s, you can transform every letter individually to be lowercase or uppercase to create another string.

Return a list of all possible strings we could create. Return the output in any order.

 
```
Example 1:

Input: s = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]
```
两个方法都是dfs而没有回溯  对比两种写法（我指dfs的参数）
## Solution 1
技巧：使用异或运算转换字母大小写
我们发现大写字符与其对应的小写字符的 ASCII 的差为 32，32 这个值如果敏感的话，它是 2^5 ，在编程语言中，可以表示为 1 << 5。而

变换大小写这件事等价于：

如果字符是小写字符，减去 32 得到大写字符；
如果字符是大写字符，加上 32 得到小写字符。
而这两者合并起来，就是给这个字符做一次不进位的加法，即异或上 1 << 5。
```java
import java.util.ArrayList;
import java.util.List;

public class Solution {

    public List<String> letterCasePermutation(String S) {
        List<String> res = new ArrayList<>();
        // convert to array
        char[] charArray = S.toCharArray();
        dfs(charArray, 0, res);
        return res;
    }

    private void dfs(char[] charArray, int index, List<String> res) {
        if (index == charArray.length) {
            res.add(new String(charArray));
            return;
        }

        dfs(charArray, index + 1, res);
        if (Character.isLetter(charArray[index])) {
            charArray[index] ^= 1 << 5;
            dfs(charArray, index + 1, res);
        }
    }
}

```


## Solution 2

```java
class Solution {
    void dfs(char[] s, int i, List<String> l){
        for(; i < s.length; i++){
            if(s[i] >= 'a' && s[i] <= 'z'){
                s[i] = (char)(s[i] - 'a' + 'A');
                dfs(s, i + 1, l);
                s[i] = (char)(s[i] - 'A' + 'a');
            }
            if(s[i] >= 'A' && s[i] <= 'Z'){
                s[i] = (char)(s[i] - 'A' + 'a');
                dfs(s, i + 1, l);
                s[i] = (char)(s[i] - 'a' + 'A');
            }
        }
        l.add(new String(s));
    }
    
    public List<String> letterCasePermutation(String S) {
        List<String> l = new ArrayList<>();
        dfs(S.toCharArray(), 0, l);
        return l;
    }
}
```