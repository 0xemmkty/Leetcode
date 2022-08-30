# [LeetCode] 253. Meeting Rooms II
Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.

Example 1:
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2

Example 2:
Input: intervals = [[7,10],[2,4]]
Output: 1
Constraints:

1 <= intervals.length <= 104
0 <= starti < endi <= 106

## Solution 1
>两种思路，我这里分享一个官方题解，有图示帮助理解。第一种是把每个会议的开始时间和结束时间拎出来分别排序，再遍历intervals。遍历的时候，设置一个pointer = 0，判断当前interval[i]的开始时间是否大于前一个会议的结束时间interval[pointer][end]。如果大于，就说明不需要新开一个会议室；如果小于，就需要新开一个会议室。因为题目只在意需要同时开几个会议室，所以start和end可以分别排序。end只要结束一个，就意味着同时需要的会议室就少一个。

**时间O(nlogn)** - 因为有对input排序

**空间O(n)** - 有用额外数组对start和end排序
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        // corner case
        if (intervals == null || intervals.length == 0) {
            return 0;
        }
        // normal case
        int len = intervals.length;
        int[] start = new int[len];
        int[] end = new int[len];
        for (int i = 0; i < len; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        Arrays.sort(start);
        Arrays.sort(end);
        int res = 0;
        int pointer = 0;
        for (int i = 0; i < len; i++) {
            if (start[i] < end[pointer]) {
                res++;
            } else {
                pointer++;
            }
        }
        return res;
    }
}
//这玩意只能记了？？想不出合适的解释
//只能说如果start<end 那就需要一个房间
//慢慢增加start 和 end
```

## Solution 2
>先对intervals按开始时间排序，再用一个最小堆存放每个interval的结束时间，并加入第一个interval，此时堆顶是第一个interval的结束时间。遍历intervals，判断当前interval的开始时间（A）跟堆顶interval的结束时间（B），若A >= B则说明两个interval没有overlap，extend堆顶interval的结束时间；若A < B则将当前interval放入堆中。

>**时间O(nlogn)**，其中排序O(nlogn)，heap的各项操作O(logn)
**空间O(n)**
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        // corner case
        if (intervals == null || intervals.length == 0) {
            return 0;
        }

        // normal case
        // sort by starting time
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        // sort by ending time
        PriorityQueue<int[]> heap = new PriorityQueue<>(intervals.length, (a, b) -> a[1] - b[1]);
        heap.offer(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            int[] now = heap.poll();
            if (intervals[i][0] >= now[1]) {
                now[1] = intervals[i][1];
            } else {
                heap.offer(intervals[i]);
            }
            heap.offer(now);
        }
        return heap.size();
    }
}
//Arrays.sort(intervals, (a, b) -> a[0] - b[0]); 根据第一个值升序排序
// 队列的add()方法和offer()方法的区别：
//两者都是往队列尾部插入元素，不同的时候，当超出队列界限的时候，
//add（）方法是抛出异常让你处理，
//offer（）方法是直接返回false
// poll 取出（会删除）
//还得再研究
```