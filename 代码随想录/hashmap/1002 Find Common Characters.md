# 1002. Find Common Characters

>Given a string array words, return an array of all characters that show up in all strings within the words (including duplicates). You may return the answer in any order.

```
Example 1:

Input: words = ["bella","label","roller"]
Output: ["e","l","l"]
```

关于这个要求，也就是说，一个字符在每一个字符串中显示的次数大小不一，但是在某一个字符串中出现的次数最小（包含相等的情况）。那么，在最终的答案中，就输出最小次数个该字符。

根据上面的说明，这里再利用题目中的示例 2 简单说下：

字符 'c'，在 'cool'，'lock'， 'cook' 三个字符串中都只出现 1 次（这就是前面所说的相等且最小的情况），在输出列表中 'c' 最终输出 1 次；
字符 'o'，在 'cool'，'cook' 中都出现了 2 次，但是在 'lock' 中只出现了 1 次，所以输出列表中，只输出 1 次 'o'。
理清题意后，我们发现可以先统计字符串数组中每个字符出现的次数，然后比较得出字符在每个字符串中出现次数最小的部分，然后将其输出。

（所以出现0次就输出0）

## Solution 1 （哈希）
```java
class Solution {
    public List<String> commonChars(String[] A) {
        List<String> result = new ArrayList<>();
        if (A.length == 0) return result;
        int[] hash= new int[26]; // 用来统计所有字符串里字符出现的最小频率
        for (int i = 0; i < A[0].length(); i++) { // 用第一个字符串给hash初始化
            hash[A[0].charAt(i)- 'a']++;
        }
        // 统计除第一个字符串外字符的出现频率
        for (int i = 1; i < A.length; i++) {
            int[] hashOtherStr= new int[26];
            for (int j = 0; j < A[i].length(); j++) {
                hashOtherStr[A[i].charAt(j)- 'a']++;
            }
            // 更新hash，保证hash里统计26个字符在所有字符串里出现的最小次数
            for (int k = 0; k < 26; k++) {
                hash[k] = Math.min(hash[k], hashOtherStr[k]);
            }
        }
        // 将hash统计的字符次数，转成输出形式
        for (int i = 0; i < 26; i++) {
            // 注意这里是while，多个重复的字符  
            // 即如果有两个l，就会输出两次
            while (hash[i] != 0) { 
                char c= (char) (i+'a');
                result.add(String.valueOf(c));
                hash[i]--;
            }
        }
        return result;
    }
}
```

## Solution 2 统计 
```java
public class Solution {
    public List<String> commonChars(String[] A) {
        // 思路：统计每个字符出现次数，有一个单词未出现就置0输出所有单词中出现0次以上】
        // 存放26个字符在单词列表的每个单词中都出现的频率
        int[] minFreq = new int[26];
        // 填充max方便下降比较
        // 降到0就最小，说明该字符在某个单词中未出现，结果中也就不需要输出
        Arrays.fill(minFreq, Integer.MAX_VALUE);
        // 遍历统计
        for (String word : A) {
            // 记录每个单词中各字符出现情况
            int[] freq = new int[26];
            int length = word.length();
            // 遍历单词
            for (int i = 0; i < length; i++) {
                // 统计某单词中字符出现情况
                char ch = word.charAt(i);
                // 累加字符出现次数
                ++freq[ch - 'a'];
            }
            // 统计完每个单词都去更新每个单词的最低出现频率
            for (int i = 0; i < 26; i++) {
                // 更新每个单词中每个字符出现的最少次数
                // 如果某个字符只在单个单词中出现自然这个字符计数会被置为0
                minFreq[i] = Math.min(minFreq[i], freq[i]);
            }
        }
        List<String> ans = new ArrayList<>();
        // 汇总输出结果
        for (int i = 0; i < 26; i++) {
            // 如果某字符只在某个单词中出现或没出现那么频率会被将为0不被计入结果
            // 其他在每个单词中都出现的字符计数多次
            for (int j = 0; j < minFreq[i]; j++) {
                ans.add(String.valueOf((char) (i + 'a')));
            }
        }
        return ans;

    }
}
```
