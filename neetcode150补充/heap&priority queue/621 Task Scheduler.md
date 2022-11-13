# 621. Task Scheduler

>Given a characters array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

>However, there is a non-negative integer n that represents the cooldown period between two same tasks (the same letter in the array), that is that **there must be at least n units of time between any two same tasks**.

>Return the least number of units of times that the CPU will take to finish all the given tasks.

## Solution 1 （构造法）
```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] cnts = new int[26];
        for (char c : tasks) cnts[c - 'A']++;
        int max = 0, tot = 0;
        // 找到最大数量的任务
        for (int i = 0; i < 26; i++) max = Math.max(max, cnts[i]);
        // 如果是max就不用加数量（因为已经放前面了）
        for (int i = 0; i < 26; i++) tot += max == cnts[i] ? 1 : 0;
        return Math.max(tasks.length, (n + 1) * (max - 1) + tot);
    }
}
// 非常漂亮的对题的分解
// 列表格图，以最大的任务数为max作为第一列，n为空闲时间加在每一行后面，一共的时间就是（max-1）*（n+1），然后最后一行再加上其他任务数
// 整个题解可以通过我自己想想图做出来
// 赔偿漂亮的分解和重构
// max(task.length,(n+1)×(max−1)+tot)

```
时间复杂度：O(n+C)O(n + C)O(n+C)
空间复杂度：O(C)O(C)O(C)，其中 C=26C = 26C=26 为任务字符集大小
