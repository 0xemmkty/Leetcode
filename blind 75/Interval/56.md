# 56. Merge Intervals

>Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

 ## Solution 1
>**时间复杂度**：O(nlogn)，其中 n 为区间的数量。除去排序的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的O(nlogn)。
**空间复杂度**：O(logn)，其中 n 为区间的数量。这里计算的是存储答案之外，使用的额外空间。O(logn) 即为排序所需要的空间复杂度。

>Java 版本献上：
对起点和终点分别进行排序，将起点和终点一一对应形成一个数组。
如果没有overlap,返回当前起点和终点
如果有overlap,判断以下条件
找出最小的起点
如果当前终点大于下一个数组的起点的时候，比较当前终点和下一个终点的大小，取为right
返回满足要求的区间[[left,right]]

 ```java
 class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> res = new ArrayList<>();
        if (intervals.length == 0 || intervals == null) return res.toArray(new int[0][]);
        // 对起点终点进行排序
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int i = 0;
        while (i < intervals.length) {
            int left = intervals[i][0];
            int right = intervals[i][1];
            // 如果有重叠，循环判断哪个起点满足条件
            while (i < intervals.length - 1 && intervals[i + 1][0] <= right) {
                i++;
                right = Math.max(right, intervals[i][1]);
            }
            // 将现在的区间放进res里面
            res.add(new int[]{left, right});
            // 接着判断下一个区间
            i++;
        }
        return res.toArray(new int[0][]);
    }
}
//思路很清晰，留意一下Arrays.sort(intervals, (a, b) -> a[0] - b[0]);排序的表达
//还有后面两个res.add(new int[]{left, right});
//和return res.toArray(new int[0][]);
```