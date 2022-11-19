# 763. Partition Labels

>You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

>Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.

>Return a list of integers representing the size of these parts.

字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

## Solution 1( greedy + double pointer )
```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        // corner case
        if (S == null || S.length() == 0) {
            return new ArrayList<>();
        }

        // normal case
        int[] last = new int[26];
        for (int i = 0; i < S.length(); i++) {
            last[S.charAt(i) - 'a'] = i;
        }
        int start = 0;
        int end = 0;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < S.length(); i++) {
            end = Math.max(end, last[S.charAt(i) - 'a']);
            if (end == i) {
                res.add(i - start + 1);
                start = i + 1;
            }
        }
        return res;
    }
}
```
时间O(n)

空间O(n)

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> list = new LinkedList<>();
        int[] edge = new int[123];
        char[] chars = S.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            edge[chars[i] - 0] = i;
        }
        int idx = 0;
        int last = -1;
        for (int i = 0; i < chars.length; i++) {
            idx = Math.max(idx,edge[chars[i] - 0]);
            if (i == idx) {
                list.add(i - last);
                last = i;
            }
        }
        return list;
    }
}
```