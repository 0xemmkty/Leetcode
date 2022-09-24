# 55. Jump Game

>You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

>Return true if you can reach the last index, or false otherwise.

```
Example 2:

Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

## Solution 1
```java
class Solution {
    public boolean canJump(int[] nums) {
        // 记录能跳到的【最远距离】 max
        int max = 0;
        for (int i = 0; i < nums.length ; i++) { // 最终 i == nums.length - 1
            // 如果i > max，说明这个位置跳不到，return false，可能中间就return false了
            if (i > max) {
                return false;
            }
            max = Math.max(max, i + nums[i]); // 更新最远距离
        }
        return true;
    }
}
```
```java
class Solution {
    public static boolean canJump(int[] nums) {
        if (nums == null) {
            return false;
        }
        //前n-1个元素能够跳到的最远距离
        int k = 0;
        for (int i = 0; i <= k; i++) {
            //第i个元素能够跳到的最远距离
            int temp = i + nums[i];
            //更新最远距离
            k = Math.max(k, temp);
            //如果最远距离已经大于或等于最后一个元素的下标,则说明能跳过去,退出. 减少循环
            if (k >= nums.length - 1) {
                return true;
            }
        }
        //最远距离k不再改变,且没有到末尾元素
        return false;
    }
}
```