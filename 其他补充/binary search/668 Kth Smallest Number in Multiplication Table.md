# 668. Kth Smallest Number in Multiplication Table

>Nearly everyone has used the Multiplication Table. The multiplication table of size m x n is an integer matrix mat where mat[i][j] == i * j (1-indexed).

>Given three integers m, n, and k, return the kth smallest element in the m x n multiplication table.

## Solution 1 (binary search)
```java
class Solution {
    public int findKthNumber(int m, int n, int k) {
        int left = 1;
        int right = m * n;
        // [left, right]
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (count(m, n, mid) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
    
    // 统计乘法表中有多少个小于等于 k 的数目
    int count(int m, int n, int k) {
        int res = 0;
        // 统计每行小于等于 k 的数目
        for (int i = 1; i <= m; ++i) {
            res += Math.min(k / i, n);
        }
        return res;
    }
}
```
时间复杂度：O(m∗log(m∗n))。
空间复杂度：O(1)。
因此当搜索空间中有多个符合要求的结果时（如 4 和 5），我们必须要选最小的数字，即 4。

为什么是最小的数字？

正是由于后面的数字在乘法表中不存在，才会导致小于等于 xxx 的数字个数存在重复！

比如，数字 5 在乘法表中不存在，所以在乘法表中小于等于 555 的数字个数 才与 小于等于 4 的数字个数相等。

作者：负雪明烛
链接：https://leetcode.cn/problems/kth-smallest-number-in-multiplication-table/solutions/1502514/by-fuxuemingzhu-8eq4/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
