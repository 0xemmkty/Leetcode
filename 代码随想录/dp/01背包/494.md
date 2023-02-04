# 494. Target Sum
Medium
8.8K
312
Companies
You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.

For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
Return the number of different expressions that you can build, which evaluates to target.

```
Example 1:

Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

## solution 1()
弄懂题意
第一步需要根据题意考虑输出的结构，在【416.分割等和子集】这道题中，要求的输出结果就是boolean值，因此我们定义的dp数组只需要记录T/F即可，但是这道题要求返回结果是方法数，那么我们dp数组需要记录的数据就是具体的方法数。

定义状态
搞清楚需要输出的结果后，就可以来想办法画一个表格，也就是定义dp数组的含义。根据背包问题的经验，可以将dp[ i ][ j ]定义为从数组nums中 0 - i 的元素进行加减可以得到 j 的方法数量。

状态转移方程
搞清楚状态以后，我们就可以根据状态去考虑如何根据子问题的转移从而得到整体的解。这道题的关键不是nums[i]的选与不选，而是nums[i]是加还是减，那么我们就可以将方程定义为：

dp[ i ][ j ] = dp[ i - 1 ][ j - nums[ i ] ] + dp[ i - 1 ][ j + nums[ i ] ]

作者：keepal
链接：https://leetcode.cn/problems/target-sum/solution/dong-tai-gui-hua-si-kao-quan-guo-cheng-by-keepal/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) sum += nums[i];
	//如果target过大 sum将无法满足
        if ( target < 0 && sum < -target) return 0;
        if ((target + sum) % 2 != 0) return 0;
        int size = (target + sum) / 2;
        if(size < 0) size = -size;
        int[] dp = new int[size + 1];
        dp[0] = 1;
        for (int i = 0; i < nums.length; i++) {
            for (int j = size; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[size];
    }
}
```
 