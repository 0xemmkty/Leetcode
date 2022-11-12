# 875. Koko Eating Bananas

>Koko loves to eat bananas. There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours.

>Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour.

>Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

>Return the minimum integer k such that she can eat all the bananas within h hours.

## Solution 1 (binary search)
本题重点：
怎么理解题目？
怎么将题目分析成二分法。
向上取整的写法

二分

由于存在「吃完这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉」的条件，因此不会存在多堆香蕉共用一个小时的情况，即每堆香蕉都是相互独立，
```java
public class Solution {

    public int minEatingSpeed(int[] piles, int H) {
        int maxVal = 1;
        for (int pile : piles) {
            maxVal = Math.max(maxVal, pile);
        }

        // 速度最小的时候，耗时最长
        int left = 1;
        // 速度最大的时候，耗时最短
        int right = maxVal;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (calculateSum(piles, mid) > H) {
                // 耗时太多，说明速度太慢了，下一轮搜索区间是 [mid + 1..right]
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }

    /**
     * 如果返回的小时数严格大于 H，就不符合题意
     *
     * @param piles
     * @param speed
     * @return 需要的小时数
     */
    private int calculateSum(int[] piles, int speed) {
        int sum = 0;
        for (int pile : piles) {
            // 上取整可以这样写
            sum += pile % speed == 0 ? pile / speed : pile / speed + 1;
            
        }
        return sum;
    }
}
```
```
时间复杂度：O(Nlogmax(piles))，这里 N 表示数组 piles 的长度。我们在 [1,maxpiles] 里使用二分查找定位最小速度，而每一次执行判别函数的时间复杂度是 O(N)；
空间复杂度：O(1)，算法只使用了常数个临时变量。
```