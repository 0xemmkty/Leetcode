#152. Maximum Product Subarray

>Given an integer array nums, find a contiguous non-empty subarray within the array that has the largest product, and return the product.
The test cases are generated so that the answer will fit in a 32-bit integer.
A subarray is a contiguous subsequence of the array.

##Solution 1 (DP)

解题思路
标签：动态规划
遍历数组时计算当前最大值，不断更新
令imax为当前最大值，则当前最大值为 imax = max(imax * nums[i], nums[i])
**由于存在负数，那么会导致最大的变最小的，最小的变最大的。因此还需要维护当前最小值**imin，imin = min(imin * nums[i], nums[i])
**当负数出现时则imax与imin进行交换**再进行下一步计算
时间复杂度：O(n)



    class Solution {
    public int maxProduct(int[] nums) {
        int max = Integer.MIN_VALUE, imax = 1, imin = 1;
        for(int i=0; i<nums.length; i++){
            if(nums[i] < 0){ 
              int tmp = imax;
              imax = imin;
              imin = tmp;
            }
            imax = Math.max(imax*nums[i], nums[i]);
            imin = Math.min(imin*nums[i], nums[i]);
            
            max = Math.max(max, imax);
        }
        return max;
    }
    }

同解注释版本

    class Solution {
    public int maxProduct(int[] nums) {
        int max=Integer.MIN_VALUE;//结果最大值
        int imax=1;int imin=1;//阶段最大值 阶段最小值
        for(int tmp:nums)
        {
            //当遇到负数的时候进行交换，因为阶段最小*负数就变阶段最大了，反之同理
            if(tmp<0){
                int exc=imax;imax=imin;imin=exc;
            }
            //在这里用乘积和元素本身比较的意思是：
            //对于最小值来说，最小值是本身则说明这个元素值比前面连续子数组的最小值还小。相当于重置了阶段最小值的起始位置
            imax=Math.max(imax*tmp,tmp);
            imin=Math.min(imin*tmp,tmp);
            //对比阶段最大值和结果最大值
            max=Math.max(imax,max);
        }
        return max;
    }
    }