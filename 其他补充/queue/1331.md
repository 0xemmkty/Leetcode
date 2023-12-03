1331. Rank Transform of an Array
Easy
1.6K
77
Companies
Given an array of integers arr, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

Rank is an integer starting from 1.
The larger the element, the larger the rank. If two elements are equal, their rank must be the same.
Rank should be as small as possible.

```python
class Solution(object):
    def arrayRankTransform(self, arr):
        """
        :type arr: List[int]
        :rtype: List[int]
        """
        dict = {}
        rank=1
        sorted_arr = sorted(set(arr))
        for i in sorted_arr:
            dict[i]=rank
            rank+=1
        ans =[]
        for i in arr:
            ans.append(dict[i])
        return ans
    


```