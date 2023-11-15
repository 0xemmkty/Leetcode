713. Subarray Product Less Than K
Medium
5.9K
180
Companies
Given an array of integers nums and an integer k, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than k.

```python
class Solution(object):
    def numSubarrayProductLessThanK(self, nums, k):
        if k <= 1:  # Handling the edge case where k is less than or equal to 1
            return 0

        res = 0
        product = 1
        left = 0
        for right in range(len(nums)):
            product *= nums[right]
            while product >= k:
                product /= nums[left]
                left += 1
            res += right - left + 1  # Count all subarrays ending at 'right'

        return res

```
# 我的错误写法
```python
class Solution(object):
    def numSubarrayProductLessThanK(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        res = 0
        p=1
        left=0
        for i in range(len(nums)):
            p = p*nums[i]
            if p<k:
                res+=1
            else:
                p=p/nums[left]
                left -=1
            return res

```


 