# [LeetCode] 252. Meeting Rooms
>Given an array of meeting time intervals where intervals[i] = [starti, endi], **determine if a person could attend all meetings**.

>Example 1:
Input: intervals = [[0,30],[5,10],[15,20]]
Output: false

>Example 2:
Input: intervals = [[7,10],[2,4]]
Output: true

>**Constraints**:
0 <= intervals.length <= 104
intervals[i].length == 2
0 <= starti < endi <= 106

## Solution 1
>给的input是一个二维数组，二维数组里面的每一个元素记录了每个会议的开始时间和结束时间。问同一个人是否有可能参加所有的会议。思路很简单，但是这是LC上算是一种比较特别的题型 - **扫描线**。这题的思路是**按照会议开始时间给input排序**，如果有任何一个会议的结束时间 > 下一个会议的开始时间，就 return false。

>**时间O(nlogn)**
**空间O(1)**

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < intervals[i - 1][1]) {
                return false;
            }
        }
        return true;
    }
}
//Arrays.sort(intervals, (a, b) -> a[1] - b[1]) 是按a从小到大排序
```
