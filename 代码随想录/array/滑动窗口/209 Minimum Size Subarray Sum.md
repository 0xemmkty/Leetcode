# 209. Minimum Size Subarray Sum

>Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray
 whose sum is **greater than or equal** to target. If there is no such subarray, return 0 instead.

 
```
Example 1:

Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

## Solution 1 (滑动窗口)
```java
class Solution {

    // 滑动窗口
    public int minSubArrayLen(int s, int[] nums) {
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;
        for (int right = 0; right < nums.length; right++) {
            // 滑动窗口的本质，扩大与缩小
            sum += nums[right];
            // 题目：greater than or equal
            while (sum >= s) {
                // 不断更新result
                result = Math.min(result, right - left + 1);
                sum -= nums[left++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```

时间复杂度：O(n)
空间复杂度：O(1)

```python
class Solution(object):
    def minSubArrayLen(self, target, nums):
        """
        :type target: int
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        sum = 0
        result = float('inf')  # Equivalent to Integer.MAX_VALUE in Java
        for right in range(len(nums)):
            sum += nums[right]
            while sum >= target:
                result = min(result, right - left + 1)
                sum -= nums[left]
                left += 1
        return 0 if result == float('inf') else result
        # 用min的方式存的是record
        # 注意题目要求， 返回的是长度

```