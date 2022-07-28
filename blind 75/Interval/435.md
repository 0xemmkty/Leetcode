# 435. Non-overlapping Intervals

>Given an array of intervals intervals where intervals[i] = [starti, endi], return the **minimum number of intervals you need to remove** to make the rest of the intervals non-overlapping.

## Solution 1 （贪心）
(注意题目要返回什么，怎么求要返回的东西。这题与之前的有所不同：之前要返回列表，这题是返回数量，而且不是重叠的数量，是移除的数量，便可以使用间接求法)
>**首个区间就是所有可以选择的区间中右端点最小的那个区间**。因此我们将所有区间按照**右端点**从小到大进行排序，那么排完序之后的首个区间，就是我们选择的首个区间。
如果有多个区间的右端点都同样最小怎么办？由于我们选择的是首个区间，因此在左侧不会有其它的区间，那么左端点在何处是不重要的，我们只要任意选择一个右端点最小的区间即可。




```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0) {
            return 0;
        }
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        int n = intervals.length;
        int right=intervals[0][1];
        int ans=1;
        for (int i = 1; i < n; i++) {
            if(intervals[i][0]>=right){
                ans++;
                right=intervals[i][1];
            }
        }
        return n-ans;
    }

}
// 按右端点排序
```