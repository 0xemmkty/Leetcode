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
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
       //判断是否为空字符串数组
        if(strs == null || strs.length == 0){
            return new ArrayList();
        }
        //1.创建一个哈希表
        Map<String,List> map = new HashMap<String, List>();
        for (String s: strs) {
            //将字符串转化为字符数组
            char[] chars = s.toCharArray();
            //对字符数组按照字母顺序排序
            Arrays.sort(chars);
            //将排序后的字符串作为哈希表中的key值
            String key = String.valueOf(chars);
            //2.判读哈希表中是否有该key值
            if (!map.containsKey(key)){
                //若不存在，则为新的异位词语，在map中创建新的键值对
                map.put(key,new ArrayList());
            }
            //3.将该字符串放在对应key的list中
            map.get(key).add(s);
        }
        //返回map中所有键值对象构成的list
        return new ArrayList(map.values());
    }
}

作者：Lan
链接：https://leetcode.cn/problems/group-anagrams/solutions/890807/zi-mu-yi-wei-fen-zu-ha-xi-biao-zhu-bu-zh-gf9q/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
```java
给老哥点个赞，我在 你代码的 Arrays.sort(chars); 优化了一下，毕竟排序复杂度为nlogn，我加了一个计数数组，然后那里的nlogn就变成了一个for循环，时间比你快1ms，哈哈哈！！！

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String,List> map=new HashMap<>();
        for(String s:strs){
            int[] s_cnt=new int[26];
            for(char ch : s.toCharArray()){
                s_cnt[ch-'a']++;
            }

            String key=new String(s_cnt,0,s_cnt.length);
           
            if(!map.containsKey(key)){
                map.put(key,new ArrayList());
            }
            map.get(key).add(s);
        }
        return new ArrayList(map.values());
    }
}
```


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

