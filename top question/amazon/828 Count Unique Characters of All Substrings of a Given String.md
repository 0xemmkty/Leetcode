# 828. Count Unique Characters of All Substrings of a Given String

Let's define a function countUniqueChars(s) that returns the number of unique characters on s.

For example, calling countUniqueChars(s) if s = "LEETCODE" then "L", "T", "C", "O", "D" are the unique characters since they appear only once in s, therefore countUniqueChars(s) = 5.
Given a string s, return the sum of countUniqueChars(t) where t is a substring of s. The test cases are generated such that the answer fits in a 32-bit integer.

Notice that some substrings can be repeated so in this case you have to count the repeated ones too.

 
```
Example 1:

Input: s = "ABC"
Output: 10
Explanation: All possible substrings are: "A","B","C","AB","BC" and "ABC".
Every substring is composed with only unique letters.
Sum of lengths of all substring is 1 + 1 + 1 + 2 + 2 + 3 = 10
Example 2:

Input: s = "ABA"
Output: 8
Explanation: The same as example 1, except countUniqueChars("ABA") = 1.
```

优秀详解：https://leetcode.cn/problems/count-unique-characters-of-all-substrings-of-a-given-string/solution/by-muse-77-v7cs/
见详解，主要是找到关系的公式
## Solution 1（hashmap）
```java
class Solution {
    public int uniqueLetterString(String s) {
        Map<Character, List<Integer>> map = new HashMap();
        char[] sc = s.toCharArray();
        for (int i = 0; i < sc.length; i++) {
            if (!map.containsKey(sc[i])) map.put(sc[i], new ArrayList());
            map.get(sc[i]).add(i);
        }

        int result = 0;
        for(Map.Entry<Character, List<Integer>> entry : map.entrySet()) {
            int head = -1, tail = -1;
            List<Integer> item = entry.getValue();
            for (int i = 0; i < item.size(); i++) {
                tail = (i < item.size() - 1) ? item.get(i + 1) : sc.length;
                result += (item.get(i) - head) * (tail - item.get(i));
                head = item.get(i);
            }
        }
        return result;
    }
}
```

## Solution2 （array）
```java
class Solution {
    public int uniqueLetterString(String s) {
        int ans = 0;
        int[] tmp0 = new int[26]; //存储某一个字符前一个字符所在位置
        int[] tmp1 = new int[26]; //存储某个字符当前所处位置

        Arrays.fill(tmp0, -1);
        Arrays.fill(tmp1, -1);

        char[] chars = s.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            char now = chars[i];
            int index = now - 'A';
            if (tmp1[index] > -1) {
                ans += (i - tmp1[index]) * (tmp1[index] - tmp0[index]);
            }
            tmp0[index] = tmp1[index];
            tmp1[index] = i;
        }
        for (int i = 0; i < 26; i++) {
            if (tmp1[i] > -1) {
                ans += (tmp1[i] - tmp0[i]) * (s.length() - tmp1[i]);
            }
        }
        return ans;
    }
}

```