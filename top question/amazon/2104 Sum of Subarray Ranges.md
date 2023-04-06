## 2104. Sum of Subarray Ranges
```
You are given an integer array nums. The range of a subarray of nums is the difference between the largest and smallest element in the subarray.

Return the sum of all subarray ranges of nums.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [1,2,3]
Output: 4
Explanation: The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0 
[2], range = 2 - 2 = 0
[3], range = 3 - 3 = 0
[1,2], range = 2 - 1 = 1
[2,3], range = 3 - 2 = 1
[1,2,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.
```

## solution 1 (枚举)

java:
1、枚举子数组

时间：O(n^2) ， Java：17ms，92%
空间：O(1)

```java
    public static long subArrayRanges(int[] arr) {
        int n = arr.length;
        long ans = 0;
        for (int i = 0; i < n - 1; i++) {
            int min = arr[i], max = arr[i];
            for (int j = i+1; j < n; j++) {
                min = Math.min(min, arr[j]);
                max = Math.max(max, arr[j]);
                ans += max - min;
            }
        }
        return ans;
    }
```

python:
```python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for i in range(n):
            minVal, maxVal = inf, -inf
            for j in range(i, n):
                minVal = min(minVal, nums[j])
                maxVal = max(maxVal, nums[j])
                ans += maxVal - minVal
        return ans

```

## Solution 2 (单调栈)
方法二：单调栈
思路与算法

为了使子数组的最小值或最大值唯一，我们定义如果 
nums[i]=nums[j]，那么 nums[i] 与 
nums[j] 的逻辑大小由下标 i 与下标 j 的逻辑大小决定，即如果 i<j，那么 
nums[i] 逻辑上小于 nums[j]。

#### 根据范围和的定义，可以推出范围和 sum等于所有子数组的最大值之和 sumMax 减去所有子数组的最小值之和 sumMin


作者：LeetCode-Solution
链接：https://leetcode.cn/problems/sum-of-subarray-ranges/solution/zi-shu-zu-fan-wei-he-by-leetcode-solutio-lamr/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```java
class Solution {
    public long subArrayRanges(int[] nums) {
        int n = nums.length;
        int[] minLeft = new int[n];
        int[] minRight = new int[n];
        int[] maxLeft = new int[n];
        int[] maxRight = new int[n];
        Deque<Integer> minStack = new ArrayDeque<Integer>();
        Deque<Integer> maxStack = new ArrayDeque<Integer>();
        /*
        This block of code uses two stacks (minStack and maxStack)
         and two arrays (minLeft and maxLeft) to calculate the 
         minimum and maximum values of each subarray that includes
          the element at index i. For each i in the range 0 to n-1,
           the code repeatedly pops elements off of the stacks 
           until the top element is less than or equal to (maxStack) 
           or greater than (minStack) the current element nums[i].
            It then stores the index of the top element on the left 
            side of the current element in the corresponding array 
            (maxLeft or minLeft). Finally, it pushes the current 
            element index onto the stack.
        */
        for (int i = 0; i < n; i++) {
            while (!minStack.isEmpty() && nums[minStack.peek()] > nums[i]) {
                minStack.pop();
            }
            minLeft[i] = minStack.isEmpty() ? -1 : minStack.peek();
            minStack.push(i);
            
            // 如果 nums[maxStack.peek()] == nums[i], 那么根据定义，
            // nums[maxStack.peek()] 逻辑上小于 nums[i]，因为 maxStack.peek() < i
            while (!maxStack.isEmpty() && nums[maxStack.peek()] <= nums[i]) { 
                maxStack.pop();
            }
            maxLeft[i] = maxStack.isEmpty() ? -1 : maxStack.peek();
            maxStack.push(i);
        }
        /*
        This block of code is similar to the previous block, but it
         calculates the minimum and maximum values of each subarray 
         that ends at the element at index i. It uses two new arrays 
         (minRight and maxRight) to store the indices of the right 
         side of each subarray. The code works by looping over the 
         indices of nums in reverse order, and repeatedly pops 
         elements off of the stacks until the top element is greater 
         than or equal to (minStack) or less than (maxStack) the 
         current element nums[i]. It then stores the index of the top 
         element on the right side of the current element in the 
         corresponding array (maxRight or minRight). Finally, it 
         pushes the current element index onto the stack.
        */
        minStack.clear();
        maxStack.clear();
        for (int i = n - 1; i >= 0; i--) {
            // 如果 nums[minStack.peek()] == nums[i], 那么根据定义，
            // nums[minStack.peek()] 逻辑上大于 nums[i]，因为 minStack.peek() > i
            while (!minStack.isEmpty() && nums[minStack.peek()] >= nums[i]) { 
                minStack.pop();
            }
            minRight[i] = minStack.isEmpty() ? n : minStack.peek();
            minStack.push(i);

            while (!maxStack.isEmpty() && nums[maxStack.peek()] < nums[i]) {
                maxStack.pop();
            }
            maxRight[i] = maxStack.isEmpty() ? n : maxStack.peek();
            maxStack.push(i);
        }

        long sumMax = 0, sumMin = 0;
        for (int i = 0; i < n; i++) {
            sumMax += (long) (maxRight[i] - i) * (i - maxLeft[i]) * nums[i];
            sumMin += (long) (minRight[i] - i) * (i - minLeft[i]) * nums[i];
        }
        return sumMax - sumMin;
    }
}
```

python
```python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        n = len(nums)
        minLeft, maxLeft = [0] * n, [0] * n
        minStack, maxStack = [], []
        for i, num in enumerate(nums):
            while minStack and nums[minStack[-1]] > num:
                minStack.pop()
            minLeft[i] = minStack[-1] if minStack else -1
            minStack.append(i)

            # 如果 nums[maxStack[-1]] == num, 那么根据定义，
            # nums[maxStack[-1]] 逻辑上小于 num，因为 maxStack[-1] < i
            while maxStack and nums[maxStack[-1]] <= num:
                maxStack.pop()
            maxLeft[i] = maxStack[-1] if maxStack else -1
            maxStack.append(i)

        minRight, maxRight = [0] * n, [0] * n
        minStack, maxStack = [], []
        for i in range(n - 1, -1, -1):
            num = nums[i]
            # 如果 nums[minStack[-1]] == num, 那么根据定义，
            # nums[minStack[-1]] 逻辑上大于 num，因为 minStack[-1] > i
            while minStack and nums[minStack[-1]] >= num:
                minStack.pop()
            minRight[i] = minStack[-1] if minStack else n
            minStack.append(i)

            while maxStack and nums[maxStack[-1]] < num:
                maxStack.pop()
            maxRight[i] = maxStack[-1] if maxStack else n
            maxStack.append(i)

        sumMax, sumMin = 0, 0
        for i, num in enumerate(nums):
            sumMax += (maxRight[i] - i) * (i - maxLeft[i]) * num
            sumMin += (minRight[i] - i) * (i - minLeft[i]) * num
        return sumMax - sumMin


