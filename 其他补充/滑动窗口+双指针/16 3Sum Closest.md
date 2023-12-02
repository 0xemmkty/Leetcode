16. 3Sum Closest
Medium
10K
528
Companies
Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.
思路简单，注释写的是之前写错的地方
```python
class Solution(object):
   
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        ans = float('inf') #漏了这个
        for i in range(len(nums)-2):
             
            left = i+1
            right = len(nums)-1
            while left < right:  #漏了这个
                sum = nums[i]+nums[left]+nums[right]
                if abs(sum-target)<abs(ans-target):   # 不能用min，为什么？
                    ans = sum

                if sum < target:
                    left+=1
                elif sum > target:   # 注意 elif 而不是 if， 不然就错得离谱了，你知道为什么
                    right-=1
                else:
                    return ans  #不能写 ans=sum, 即使表达是一样的，但是在while里面执行很多次 time limit exceed
        return ans
```

            

        