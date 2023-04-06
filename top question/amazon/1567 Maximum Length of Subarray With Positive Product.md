# 1567. Maximum Length of Subarray With Positive Product

Given an array of integers nums, find the maximum length of a subarray where the product of all its elements is positive.

A subarray of an array is a consecutive sequence of zero or more values taken out of that array.

Return the maximum length of a subarray with positive product.

 ## Solution 1
 ```java
 class Solution {
    public int getMaxLen(int[] nums) {
        // 动态规划
        // 定义子问题 dpP[i] 为包含nums[i]的乘机为正的最长子数组长度，dpN[i]为乘机为负的最长子数组长度
        // 如果不存在，则定义为0，
        int size = nums.length;
        int[] dpP = new int[size+1], dpN = new int[size+1];

        for (int i=1; i<size+1; i++) {
            if (nums[i-1] > 0) {
                dpP[i] = dpP[i-1]+1;
                dpN[i] = dpN[i-1] == 0 ? 0 : dpN[i-1]+1;
            } else if (nums[i-1] == 0) {
                dpP[i] = 0;
                dpN[i] = 0;
            } else {
                dpP[i] = dpN[i-1] == 0 ? 0 : dpN[i-1]+1;
                dpN[i] = dpP[i-1]+1;
            }
        }
        int res = 0;
        for (int i=1; i<size+1; i++) {
            res = Math.max(res, dpP[i]);
        }
        return res;

    }
}
```