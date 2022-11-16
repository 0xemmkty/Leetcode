# 238. Product of Array Except Self

>Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

>The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

>You must write an algorithm that runs in O(n) time and without using the division operation.

```
Example 1:

Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

## solution 1
// h很漂亮的解法
// 相当于把一个数载array中的位置分城左部分和右部分
// 双循环 
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        int p = 1, q = 1;
        for (int i = 0; i < nums.length; i++) {
            // 求得p然后循环移到下一位
            // 下一位的res就是上一位的累乘p
            res[i] = p;
            p *= nums[i];
        }
        for (int i = nums.length - 1; i > 0 ; i--) {
            // 同p 求得q 然后往前移一位
            q *= nums[i];
            // i-1 第一次就是自动忽略了最右的一个（不需要）
            res[i - 1] *= q;
        }
        return res;
    }
}
```
o（n）
