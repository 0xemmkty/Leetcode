# 452. Minimum Number of Arrows to Burst Balloons
（不是特别理解，再看看）
>There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array points where points[i] = [xstart, xend] denotes a balloon whose horizontal diameter stretches between xstart and xend. You do not know the exact y-coordinates of the balloons.

>Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis. A balloon with xstart and xend is burst by an arrow shot at x if xstart <= x <= xend. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array points, return the minimum number of arrows that must be shot to burst all balloons.

 
```
Example 1:

Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
```

## S1 (贪心)
局部最优：当气球出现重叠，一起射，所用弓箭最少。全局最优：把所有气球射爆所用弓箭最少。
为了让气球尽可能的重叠，需要对数组进行排序。
那么按照气球起始位置排序，还是按照气球终止位置排序呢？

其实都可以！只不过对应的遍历顺序不同，我就按照气球的起始位置排序了。

既然按照起始位置排序，那么就从前向后遍历气球数组，靠左尽可能让气球重复。

从前向后遍历遇到重叠的气球了怎么办？

如果气球重叠了，重叠气球中右边边界的最小值 之前的区间一定需要一个弓箭。
```java
/**
 * 时间复杂度 : O(NlogN)  排序需要 O(NlogN) 的复杂度
 * 空间复杂度 : O(logN) java所使用的内置函数用的是快速排序需要 logN 的空间
 */
class Solution {
    public int findMinArrowShots(int[][] points) {
        // 根据气球直径的开始坐标从小到大排序
        // 使用Integer内置比较方法，不会溢出
        Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));

        int count = 1;  // points 不为空至少需要一支箭
        for (int i = 1; i < points.length; i++) {
            // points[i][0]指后一个的开始，
            // points[i-1][1]指前一个的结束
            if (points[i][0] > points[i - 1][1]) {  // 气球i和气球i-1不挨着，注意这里不是>=
                count++; // 需要一支箭
                // 注意到else时的i已经是下一个循环的i了（假设已经执行上一个if）
            } else {  // 气球i和气球i-1挨着
                points[i][1] = Math.min(points[i][1], points[i - 1][1]); // 更新重叠气球最小右边界
            }
        }
        return count;
    }
}
```
