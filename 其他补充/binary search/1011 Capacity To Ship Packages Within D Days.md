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