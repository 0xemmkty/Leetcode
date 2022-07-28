# 57. Insert Interval

>You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.
Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).
Return intervals after the insertion.

## Solution 1

>不重叠，需满足：绿区间的右端，位于蓝区间的左端的左边，如[1,2]。
则当前绿区间，推入 res 数组，指针 +1，考察下一个绿区间。循环结束时，当前绿区间的屁股，就没落在蓝区间之前，有重叠了，如[3,5]。
现在看重叠的。我们反过来想，没重叠，就要满足：绿区间的左端，落在蓝区间的屁股的后面，反之就有重叠：绿区间的左端 <= 蓝区间的右端，极端的例子就是[8,10]。
和蓝有重叠的区间，会合并成一个区间：左端取蓝绿左端的较小者，右端取蓝绿右端的较大者，不断更新给蓝区间。
循环结束时，将蓝区间（它是合并后的新区间）推入 res 数组。
剩下的，都在蓝区间右边，不重叠。不用额外判断，依次推入 res 数组。

作者：xiao_ben_zhu
链接：https://leetcode.cn/problems/insert-interval/solution/shou-hua-tu-jie-57-cha-ru-qu-jian-fen-cheng-3ge-ji/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> list = new ArrayList<>();
        if(intervals == null || intervals.length == 0) return new int[][]{newInterval};
        if(newInterval == null || newInterval.length == 0) return intervals;
        int begin = newInterval[0], end = newInterval[1];
        int i = 0;
        /** 
        将整个区间列表分为三部分：
        第一部分为待插入区间前面没有重合的所有区间
        第二部分为有重合需要合并的所有区间
        第三部分为待插入区间后面没有重合的所有区间

        第一部分和第三部分这两部分的所有区间就直接遍历加入到结果列表中即可
        主要是合并中间有重合区间，将其合并为一个新区间：
        合并规则就是左边界去这些重合区间所有左边界的最小值，右边界则是所有区间右边界的最大值
        */

        //第一部分。判断属于第一部分的条件就是待插入区间的待插入区间的左边界大于其右边界
        for(; i < intervals.length; i++){
            if(begin > intervals[i][1]){
                list.add(new int[]{intervals[i][0], intervals[i][1]});
            }else{
                break;
            }
        }
        //第二部分即有重合的部分。判断有重合的条件就是：“遍历完第一部分时已经知道待插入区间的
        //左边界值小于等于这些区间的右边界值了，只需要待插入区间的右边界值再大于等于这一部分区
        //间的左边界值就可以确定是有重合的了”
        for(; i < intervals.length; i++){
            if(end >= intervals[i][0]){
                begin = Math.min(begin, intervals[i][0]);
                end = Math.max(end, intervals[i][1]);
            }else{
                break;
            }
        }
        //第三部分。第二部分遍历完剩下的就全是第三部分的了
        list.add(new int[]{begin, end});
        for(; i < intervals.length; i++){
            list.add(new int[]{intervals[i][0], intervals[i][1]});
        }
        return list.toArray(new int[0][]);
    }
}

// first: intervals(null? length?), newintervals (null?length?)
// next, begin and end to represent the newintervals(begin and end)
// three case: 
//    intervals[i][1] means the end of intervals, we need to use begin to compare intervals[i][1]
//  if begin > intervals[i][1]
//  there is no overlap, this means we can add intervals[][] to our answer list
//notice that i in this question is not independent, they are connect.
//  i means 第几个区间
// case 2 is important, it solve the overlap interval and change the value of begin and end(merge), then add the new merged interval into answer list
//case 3 is the remain not overlap interval, we can add it to answer list directly
```