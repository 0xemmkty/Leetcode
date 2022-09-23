# 377. Combination Sum IV

>Given an array of distinct integers nums and a target integer target, return the number of possible combinations that add up to target.
The test cases are generated so that the answer can fit in a 32-bit integer.

 ```
 Example 1:

Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```

## Solution 1
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        //target 楼梯总层数
        //nums 是每次上楼梯的步数你可以选。 比如(1,2,3) 就是走1步2步3步
        int[] dp = new int[target + 1];
        dp[0] = 1;//0层的时候就只有1种走法
        //这里拿实例（1,2,3 ) t=4举例 
        //i=1 层遍历走法(1,2,3) dp[1]=dp[0]=1;
        //i=2 层遍历走法(1,2,3) dp[2]=dp[1]+dp[0]=2
        //i=3 层遍历走法(1,2,3) dp[3]=dp[2]+d[1]+dp[0]=4
        //i=4 层遍历走法(1,2,3) dp[4]=dp[3]+d[2]++dp[1]+dp[0]=7

        for (int i = 1; i <= target; i++) {
            for (int num : nums) {
                //这个地方只是过滤上楼的步子不能太大会扯着蛋。只能在<=终点（也是i）的步伐里面选择前进方式
                if (num <= i) {
                    dp[i] += dp[i - num];
                }
            }
        }

        return dp[target];
    }
}
// image you are climb stair
// i represent the ith level stair
// i - num means in fact there still how many stair you need to climb
// let big problem break down to the small one
// interesting!!
```

时间复杂度：O(t∗n)
空间复杂度：O(t)