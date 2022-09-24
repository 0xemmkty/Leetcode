# 139. Word Break (欠 s2和s3)

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

```
Example 2:

Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

## Solution 1 (DP)
方法3：动态规划
s 串能否分解为单词表的单词（前 s.length 个字符的 s 串能否分解为单词表单词）
将大问题分解为规模小一点的子问题：
前 ii 个字符的子串，能否分解成单词
剩余子串，是否为单个单词。
dp[i]：长度为i的s[0:i-1]子串是否能拆分成单词。题目求:dp[s.length]

**状态转移方程**
类似的，我们用指针 j 去划分s[0:i] 子串，如下图：
s[0:i] 子串对应 dp[i+1] ，它是否为 true（s[0:i]能否 break），**取决于两点**：
它的前缀子串 s[0:j-1] 的 **dp[j]，是否为 true**。
**剩余子串 s[j:i]，是否是单词表的单词**。

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int maxWordLength = 0;
        Set<String> wordSet = new HashSet<>(wordDict.size());
        for (String word : wordDict) {
            wordSet.add(word);

            if (word.length() > maxWordLength) {
                maxWordLength = word.length();
            }
        }

        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i < dp.length; i++) {
            for (int j = (i - maxWordLength < 0 ? 0 : i - maxWordLength); j < i; j++) {
                if (dp[j] && wordSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[dp.length - 1];
    }
}
```

## Solution 2 （DFS）
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] visited = new boolean[s.length() + 1];
        return dfs(s, 0, wordDict, visited);
    }
    private boolean dfs(String s, int start, List<String> wordDict, boolean[] visited) {
        for (String word : wordDict) {
            int nextStart = start + word.length();
            if (nextStart > s.length() || visited[nextStart]) {
                continue;
            }

            if (s.indexOf(word, start) == start) {
                if (nextStart == s.length() || dfs(s, nextStart, wordDict, visited)) {
                    return true;
                }
                visited[nextStart] = true;
            }
        }
        return false;
    }
}
```

## Solution 3 (BFS)
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(0);

        int slength = s.length();
        boolean[] visited = new boolean[slength + 1];

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int start = queue.poll().intValue();
                for (String word : wordDict) {
                    int nextStart = start + word.length();
                    if (nextStart > slength || visited[nextStart]) {
                        continue;
                    }

                    if (s.indexOf(word, start) == start) {
                        if (nextStart == slength) {
                            return true;
                        }

                        queue.add(nextStart);
                        visited[nextStart] = true;
                    }
                }
            }
        }

        return false;
    }
}

```