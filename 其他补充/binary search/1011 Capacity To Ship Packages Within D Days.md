# 1011. Capacity To Ship Packages Within D Days

>A conveyor belt has packages that must be shipped from one port to another within days days.

The ith package on the conveyor belt has a weight of weights[i]. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within days days.

```
Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15
Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.
```
## solution （精确分界二分法）
二分解法（精确边界）

假定「D 天内运送完所有包裹的最低运力」为 ans，那么在以 ans 为分割点的数轴上具有「二段性」：

数值范围在 (−∞,ans) 的运力必然「不满足」 D 天内运送完所有包裹的要求
数值范围在 [ans,+∞) 的运力必然「满足」 D天内运送完所有包裹的要求
即我们可以通过「二分」来找到恰好满足 D天内运送完所有包裹的分割点 ans。

接下来我们要确定二分的范围，由于不存在包裹拆分的情况，考虑如下两种边界情况：

理论最低运力：只确保所有包裹能够被运送，自然也包括重量最大的包裹，此时理论最低运力为 max，max 为数组 weights 中的最大值
理论最高运力：使得所有包裹在最短时间（一天）内运送完成，此时理论最高运力为 sum，sum 为数组 weights 的总和
由此，我们可以确定二分的范围为 [max,sum]。
```java
class Solution {
    public int shipWithinDays(int[] ws, int d) {
        int max = 0, sum = 0;
        for (int w : ws) {
            max = Math.max(max, w);
            sum += w;
        }
        int l = max, r = sum;
        while (l < r) {
            int mid = (l + r)/2;
            if (check(ws, mid, d)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return r;
    }
    boolean check(int[] ws, int t, int d) {
        int n = ws.length;
        int cnt = 1;
        // each time loop will set sum to 0
        // cnt 相当于第几个合适的分组，在该题里面就是day
        // 因为循环是先让cnt+1再判断，所以当判断不成功的时候，return的地方要让cnt-1
        for (int i = 1, sum = ws[0]; i < n; sum = 0, cnt++) {
            // 判断几个sum分在一组（day）满足条件（<mid）
            while (i < n && sum + ws[i] <= t) {
                sum += ws[i];
                i++;
            }
        }
        return cnt - 1 <= d;
    }
}
```
时间复杂度：二分范围为 [max,sum]，check 函数的复杂度为O(n)。整体复杂度为 O(nlog(∑ i=0 n−1 ws[i]))
空间复杂度：O(1)O(1)O(1)



```java
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        // 确定二分查找左右边界
        int left = Arrays.stream(weights).max().getAsInt(), right = Arrays.stream(weights).sum();
        while (left < right) {
            int mid = (left + right) / 2;
            // need 为需要运送的天数
            // cur 为当前这一天已经运送的包裹重量之和
            int need = 1, cur = 0;
            for (int weight : weights) {
                if (cur + weight > mid) {
                    ++need;
                    cur = 0;
                }
                cur += weight;
            }
            if (need <= days) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}



```

## Solution (粗略边界二分法)
其实就是没必要找到max和sum来做二分的范围，反正算法都是计算不差这一点时间
```
二分解法（粗略边界）

当然，一个合格的「二分范围」只需要确保包含分割点 ans 即可。因此我们可以利用数据范围来确立粗略的二分范围（从而少写一些代码）：

利用运力必然是正整数，从而确定左边界为 1
根据 1⩽D⩽weights.length⩽50000 和 1⩽weights[i]⩽500，从而确定右边界为 1e8
PS. 由于二分查找具有折半效率，因此「确立粗略二分范围」不会比「通过循环取得精确二分范围」效率低。

```
```java
class Solution {
    public int shipWithinDays(int[] ws, int d) {
        int l = 1, r = (int)1e8;
        while (l < r) {
            int mid = l + r >> 1;
            if (check(ws, mid, d)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return r;
    }
    boolean check(int[] ws, int t, int d) {
        if (ws[0] > t) return false;
        int n = ws.length;
        int cnt = 1;
        for (int i = 1, sum = ws[0]; i < n; sum = 0, cnt++) {
            if (ws[i] > t) return false;
            while (i < n && sum + ws[i] <= t) {
                sum += ws[i];
                i++;
            }
        }
        return cnt - 1 <= d;
    }
}

```