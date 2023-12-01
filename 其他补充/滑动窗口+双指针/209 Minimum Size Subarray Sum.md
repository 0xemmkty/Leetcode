# 209 Minimum Size Subarray Sum
Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray
 whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.


```python
# easy but need to consider why need use min
class Solution(object):
    def minSubArrayLen(self, target, nums):
        """
        :type target: int
        :type nums: List[int]
        :rtype: int
        """
        
        min_length = float('inf')  # Initialize min_length to infinity
        current_sum = 0  # Sum of the current window
        start = 0  # Starting index of the sliding window

        for end in range(len(nums)):
            current_sum += nums[end]

        # Shrink the window as small as possible while the sum is still >= target
            while current_sum >= target:
                min_length = min(min_length, end - start + 1)
                current_sum -= nums[start]
                start += 1

        return min_length if min_length != float('inf') else 0
        
```