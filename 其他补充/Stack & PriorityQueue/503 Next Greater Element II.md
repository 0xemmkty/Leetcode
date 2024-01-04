503. Next Greater Element II
单调栈 环形数组
下一个最大的元素是 下一个最大（空间） 而不是差值最小的数组上的下一个最大
Medium
7.6K
183
Companies
Given a circular integer array nums (i.e., the next element of nums[nums.length - 1] is nums[0]), return the next greater number for every element in nums.

The next greater number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return -1 for this number.

 

Example 1:

Input: nums = [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.

```python
class Solution(object):
    def nextGreaterElements(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        n = len(nums)
        res = [-1] * n  # Initialize the result list with -1
        stack = []  # Use a stack to store indices of elements
        
        for i in range(2 * n):
            current_index = i % n  # Get the current index in the circular array
            while stack and nums[current_index] > nums[stack[-1]]:
                # If the current element is greater than the element at the top of the stack
                prev_index = stack.pop()  # Pop the index from the stack
                res[prev_index] = nums[current_index]  # Set the result for the popped index
            
            # Push the current index onto the stack for comparison
            if i < n:
                stack.append(current_index)
        
        return res


```