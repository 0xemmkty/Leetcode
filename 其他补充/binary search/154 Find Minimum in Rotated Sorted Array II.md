# 154. Find Minimum in Rotated Sorted Array II
本题与153相似，差别是153中nums里面不含重复数组
154中nums含重复数组

>Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,4,4,5,6,7] might become:

>[4,5,6,7,0,1,4] if it was rotated 4 times.
[0,1,4,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

>Given the sorted rotated array nums that may contain duplicates, return the minimum element of this array.

>You must decrease the overall operation steps as much as possible.

## solution
但和 153. 寻找旋转排序数组中的最小值 不同的是，本题元素并不唯一。

这意味着我们无法直接根据与 nums[0]nums[0]nums[0] 的大小关系，将数组划分为两段，即无法通过「二分」来找到旋转点。

```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int l = 0, r = n - 1;
        // 比153多了这步，目的是消除与nums【0】一样的元素
        while (l < r && nums[0] == nums[r]) r--;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] >= nums[0]) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return r + 1 < n ? nums[r + 1] : nums[0];
    }
}


```
时间复杂度：恢复二段性处理中，最坏的情况下（考虑整个数组都是同一个数）复杂度是 O(n)O(n)O(n)，而之后的找旋转点是「二分」，复杂度为 O(logn)O(log{n})O(logn)。整体复杂度为 O(n)O(n)O(n) 的。
空间复杂度：O(1)O(1)O(1)。

简单