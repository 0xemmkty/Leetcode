# 1046. Last Stone Weight

>You are given an array of integers stones where stones[i] is the weight of the ith stone.

>We are playing a game with the stones. On each turn, we choose the heaviest two stones and smash them together. Suppose the heaviest two stones have weights x and y with x <= y. The result of this smash is:

>If x == y, both stones are destroyed, and
If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.
At the end of the game, there is at most one stone left.

>Return the weight of the last remaining stone. If there are no stones left, return 0.

## Solution1 (pq)
怎么做判断？ 
size 与 1 比较
```

将所有石头的重量放入最大堆中。每次依次从队列中取出最重的两块石头 aaa 和 bbb，必有 a≥ba \ge ba≥b。如果 a>ba>ba>b，则将新石头 a−ba-ba−b 放回到最大堆中；如果 a=ba=ba=b，两块石头完全被粉碎，因此不会产生新的石头。重复上述操作，直到剩下的石头少于 222 块。

最终可能剩下 111 块石头，该石头的重量即为最大堆中剩下的元素，返回该元素；也可能没有石头剩下，此时最大堆为空，返回 000。
```
```java
class Solution {
    public int lastStoneWeight(int[] stones) {
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>((a, b) -> b - a);
        for (int stone : stones) {
            pq.offer(stone);
        }

        while (pq.size() > 1) {
            int a = pq.poll();
            int b = pq.poll();
            if (a > b) {
                pq.offer(a - b);
            }
        }
        return pq.isEmpty() ? 0 : pq.poll();
    }
}


```
时间复杂度：O(nlogn)，其中 n 是石头数量。每次从队列中取出元素需要花费 O(logn) 的时间，最多共需要粉碎 n−1 次石头。

空间复杂度：O(n)。
