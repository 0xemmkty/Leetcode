# 424. Longest Repeating Character Replacement (欠)

>You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

>Return the length of the longest substring containing the same letter you can get after performing the above operations.

## Solution 1
本题可以先退化成考虑K=0 的情况，此时题目就变成了求解字符串中最长连续子串长度问题了（即力扣3）。
我们先可以通过这个特例先了解一下滑动窗口的求解过程：
增加窗口的宽度，滑动的是窗口
上图的求解过程展示中，窗口从左至右不断扩张/滑动，当窗口触达字符串末尾字符时，运算结束，窗口的宽度为最终结果。初始窗口的宽度为 1，我们不断的通过向当前窗口覆盖的子串后面追加一个字符看是否能满足我们的要求，如果满足窗口扩张，如果不满足，窗口向右滑动。

当K>0 时，子串的条件变成了允许我们变换子串中的 K 个字符使其变成一个连续子串

那么这个题的关键点就是我们如何判断一个字符串改变 K 个字符，能够变成一个连续串

**如果当前字符串中的出现次数最多的字母个数 +K 大于串长度**，那么这个串就是满足条件的

我们维护一个数组 int[26] 来存储当前窗口中各个字母的出现次数，left 表示窗口的左边界，right 表示窗口右边界

窗口扩张：left 不变，right++
窗口滑动：left++，right++
historyCharMax 保存滑动窗口内相同字母出现次数的 历史 最大值，通过判断窗口宽度 (right−left+1) 是否大于 historyCharMax+K 来决定窗口是否做滑动，否则窗口就扩张。
```java
class Solution {
    public int characterReplacement(String s, int k) {
         if(s == null){
             return 0;
         }
        int[] map = new int[26];
        char[] chars = s.toCharArray();
        int left = 0;
        int right = 0;
        int historyCharMax = 0;//滑动窗口历史上在一个窗口内出现次数最多的字母的次数（仅限在当时的窗口内出现）
        for(right = 0;right < chars.length; right++){
            int index = chars[right] - 'A';
            map[index]++;//将右指针右移后对应的新元素添加到map中
            historyCharMax = Math.max(historyCharMax, map[index]);//更新历史最大出现次数
            //如果新加元素（即新加元素就是当下出现次数最多的字母）使得当下historyCharMax增加了1，则扩大窗口，以下if不执行
            //否则，不扩窗，窗口滑动（右指针已经右移一位了，需要把左指针左移，执行if语句）
            if(right - left + 1 > historyCharMax + k){
                / 说明此时 k 不够用
                // 把其它不是最多出现的字符替换以后，都不能填满这个滑动的窗口，这个时候须要考虑左边界向右移动
                // 移出滑动窗口的时候，频数数组须要相应地做减法


                map[chars[left] - 'A']--;
                left++;
            }
        }
        return chars.length - left;//即right - left最终窗口的长度 右指针right最终一定会移到chars.length即数组的末端
    }
}
// this question can be change to "find the max num of "
// int[] initial value is 0
// int index = chars[right] - 'A';
//    map[index]++;
// this two line means that if two index are the same, then the value of map[index] may add 1
//
```

时间复杂度：O(N)，这里 N是输入字符串 S 的长度；
空间复杂度：O(A)，这里 A 是输入字符串 S 出现的字符 ASCII 值的范围


