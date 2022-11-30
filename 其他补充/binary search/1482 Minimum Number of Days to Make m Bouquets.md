# 1482. Minimum Number of Days to Make m Bouquets

You are given an integer array bloomDay, an integer m and an integer k.

You want to make m bouquets. To make a bouquet, you need to use k adjacent flowers from the garden.

The garden consists of n flowers, the ith flower will bloom in the bloomDay[i] and then can be used in exactly one bouquet.

Return the minimum number of days you need to wait to be able to make m bouquets from the garden. If it is impossible to make m bouquets return -1.

## solution 1 （二分法）
```
此题初看确实很难想到是二分查找。但题目询问最少的天数，如果给定一个天数让我们返回能否制作完成花束，是不是就有思路了。

但是如何得到一个确定的天数呢？理论的最小天数和最大天数肯定是bloomDay 的最小最大值。那么如何在这个数组里寻找到一个满足条件的天数，是不是就能想到二分了。

判断条件就是 在mid天能否完成花束，需要注意的是题目要求必须是相邻的k朵花组成一束。


题目需要求得「所需的最少天数」。

假设「所需的最少天数」为 ans ，那么以 ans 为分割点的数轴具有「二段性」：

天数范围落在 [0,ans)[0, ans)[0,ans) 无法制作完成
天数范围在 [ans,+∞)[ans, +∞)[ans,+∞) 可以制作完成
因此可以通过「二分」来找到分割点 ans。

```
（先用二分法找到mid，将mid作为day传入新方法）
```java
class Solution {
    public int minDays(int[] bloomDay, int m, int k) {

        if( m > bloomDay.length / k) return -1; // 此时无论如何都制作不出来m束花

        // 只要 m <= bloomDay.length / k, 成立，无论如何都能制作出来
        // 调用两次stream()方法求最值的效率较低，可以使用常规方法替代！
        int low = Arrays.stream(bloomDay).min().getAsInt(); // 花开的最小天数
        int high = Arrays.stream(bloomDay).max().getAsInt(); // 花开的最大天数

        // 如果可以制作m束花，天数一定在low和high之间，因此使用二分查找
        while(low < high){
            int mid = low + (high-low)/2;
            if(make(bloomDay,m,k,mid)){
                high = mid;
            }else low = mid+1;
        }
        return low;
        
    }

    public boolean make(int[] bloomDay, int m, int k,int days){
        int flowers = 0; // 代表可用的花的个数
        int makeFlowers = 0; // 代表当前天数days可以制作出的花的数量
        int n = bloomDay.length;
        for(int i = 0; i < n; i++){
            if(bloomDay[i] <= days){
                flowers++; // 只要开花所需天数小于等于days, 则说明当前花可用
                if(flowers == k){   // 当前花的数量满足可以制作一束花的数量k时，则制作出的花的数量makeFlowers++;并重置makeFlowers
                    makeFlowers++;
                    flowers = 0;  // 重置当前可用花的数量
                }
            }else flowers = 0; // 因为需要连续的k朵花，因此只要中间有一朵花没开, flowers就重置为0   
            
        }
        return makeFlowers >= m; // 只要 makeFlowers >= m 就说明可以满足要求
    }
}
```
(check版本 更快)
```java
class Solution {
    int n, m, k;
    public int minDays(int[] nums, int _m, int _k) {
        n = nums.length;
        m = _m; k = _k;

        if (n < m * k) return -1;
                
        int l = 0, r = (int)1e9;
        while (l < r) {
            int mid = l + r >> 1; // /2
            if (check(nums, mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return check(nums, r) ? r : -1;
    }
    boolean check(int[] nums, int mid) {
        int cnt = 0;
        for (int i = 0; i < n && cnt < m; ) {
            int cur = nums[i] <= mid ? 1 : 0, j = i;
            if (cur > 0) {
                while (cur < k && j + 1 < n && nums[j + 1] <= mid) {
                    j++;
                    cur++;
                }
                if (cur == k) cnt++;
                i = j + 1;
            } else {
                i++;
            }
        }
        return cnt >= m;
    }
}
时间复杂度：check 函数的复杂度为 O(n)。整体复杂度为 O(nlog1e9)
空间复杂度：O(1)



```