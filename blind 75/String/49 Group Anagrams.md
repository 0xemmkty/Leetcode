# 49. Group Anagrams

>Given an array of strings strs, group the anagrams together. You can return the answer in any order.

>An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

```
Example 1:

Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

## Solution 1
方法一：排序
由于互为字母异位词的两个字符串包含的字母相同，**因此对两个字符串分别进行排序之后得到的字符串一定是相同的**，故可以将排序之后的字符串作为哈希表的键。
思路简单
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```
时间复杂度：O(nk \log k)O(nklogk)，其中 nn 是 \textit{strs}strs 中的字符串的数量，kk 是 \textit{strs}strs 中的字符串的的最大长度。需要遍历 nn 个字符串，对于每个字符串，需要 O(k \log k)O(klogk) 的时间进行排序以及 O(1)O(1) 的时间更新哈希表，因此总时间复杂度是 O(nk \log k)O(nklogk)。

空间复杂度：O(nk)O(nk)，其中 nn 是 \textit{strs}strs 中的字符串的数量，kk 是 \textit{strs}strs 中的字符串的的最大长度。需要用哈希表存储全部字符串。



## Solution 2 (欠)
>方法二：计数
由于互为字母异位词的两个字符串包含的字母相同，因此两个字符串中的相同字母出现的次数一定是相同的，故可以将每个字母出现的次数使用字符串表示，作为哈希表的键。
>由于字符串只包含小写字母，因此对于每个字符串，可以使用长度为 2626 的数组记录每个字母出现的次数。需要注意的是，在使用数组作为哈希表的键时，不同语言的支持程度不同，因此不同语言的实现方式也不同

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            int[] counts = new int[26];
            int length = str.length();
            for (int i = 0; i < length; i++) {
                counts[str.charAt(i) - 'a']++;
            }
            // 将每个出现次数大于 0 的字母和出现次数按顺序拼接成字符串，作为哈希表的键
            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < 26; i++) {
                if (counts[i] != 0) {
                    sb.append((char) ('a' + i));
                    sb.append(counts[i]);
                }
            }
            String key = sb.toString();
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}

