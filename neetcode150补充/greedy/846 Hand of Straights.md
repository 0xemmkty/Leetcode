# 846. Hand of Straights

>Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of **size groupSize**, and consists of groupSize **consecutive cards**.

>Given an integer array hand where hand[i] is the value written on the ith card and an integer groupSize, return true if she can rearrange the cards, or false otherwise.

## Solution 1
哈希表和优先队列
```java
class Solution {
    public boolean isNStraightHand(int[] hand, int m) {
        Map<Integer, Integer> map = new HashMap<>();
        PriorityQueue<Integer> q = new PriorityQueue<>((a,b)->a-b);
        for (int i : hand) {
            map.put(i, map.getOrDefault(i, 0) + 1);
            q.add(i);
        }
        while (!q.isEmpty()) {
            int t = q.poll();
            if (map.get(t) == 0) continue;
            for (int i = 0; i < m; i++) {
                int cnt = map.getOrDefault(t + i, 0);
                if (cnt == 0) return false;
                map.put(t + i, cnt - 1);
            }
        }
        return true;
    }
}

```
时间复杂度：令 n 为数组 hand 长度，使用哈希表进行次数统计的复杂度为O(n)；将所有元素从堆中存入和取出的复杂度为 O(nlogn)。整体复杂度为 O(nlogn)
空间复杂度：O(n)

