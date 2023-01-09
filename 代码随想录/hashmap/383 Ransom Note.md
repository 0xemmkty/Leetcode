# 383. Ransom Note

Given two strings ransomNote and magazine, return true if ransomNote can be constructed by using the letters from magazine and false otherwise.

Each letter in magazine can only be used once in ransomNote.

 
```
Example 1:

Input: ransomNote = "a", magazine = "b"
Output: false
```

```
Example 3:

Input: ransomNote = "aa", magazine = "aab"
Output: true
```
## Solution 1 (hash)
方法一：字符统计
题目要求使用字符串 magazine 中的字符来构建新的字符串 ransomNote，且ransomNote 中的每个字符只能使用一次，只需要满足字符串 magazine 中的每个英文字母 (’a’-’z’) 的统计次数都大于等于 ransomNote 中相同字母的统计次数即可。

如果字符串 }magazine 的长度小于字符串ransomNote 的长度，则我们可以肯定 magazine 无法构成 ransomNote，此时直接返回 false。
首先统计 magazine 中每个英文字母 a 的次数 cnt[a]，再遍历统计 ransomNote 中每个英文字母的次数，如果发现 ransomNote 中存在某个英文字母 c 的统计次数大于 magazine 中该字母统计次数 cnt[c]，则此时我们直接返回 false。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote.length() > magazine.length()) {
            return false;
        }
        int[] cnt = new int[26];
        for (char c : magazine.toCharArray()) {
            cnt[c - 'a']++;
        }
        for (char c : ransomNote.toCharArray()) {
            cnt[c - 'a']--;
            if(cnt[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```

复杂度分析

时间复杂度：O(m+n)，其中 m 是字符串 ransomNote 的长度，nn 是字符串 magazine 的长度，我们只需要遍历两个字符一次即可。

空间复杂度：O(∣S∣)，S 是字符集，这道题中 S 为全部小写英语字母，因此 ∣S∣=26。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        // 定义一个哈希映射数组
        int[] record = new int[26];

        // 遍历
        for(char c : magazine.toCharArray()){
            record[c - 'a'] += 1;
        }

        for(char c : ransomNote.toCharArray()){
            record[c - 'a'] -= 1;
        }
        
        // 如果数组中存在负数，说明ransomNote字符串总存在magazine中没有的字符
        for(int i : record){
            if(i < 0){
                return false;
            }
        }

        return true;
    }
}
```