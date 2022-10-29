# 45. jump game II
（对比55，45是跳到最后一格，55是判断能否跳完）
You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

0 <= j <= nums[i] and
i + j < n
Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

## solution
```java
class Solution {
    public int jump(int[] nums) {
        // 记录当前能跳跃到的位置的边界下标
        int border = 0;
        // 记录在边界范围内，能跳跃的最远位置的下标
        int maxPosition = 0;
        // 记录所用步数
        int steps = 0;
        for(int i=0;i<nums.length-1;i++){
            // 继续往下遍历，统计边界范围内，哪一格能跳得更远，每走一步就更新一次能跳跃的最远位置下标
            // 其实就是在统计下一步的最优情况
            maxPosition = Math.max(maxPosition,nums[i]+i);
            // 如果到达了边界，那么一定要跳了，下一跳的边界下标就是之前统计的最优情况maxPosition，并且步数加1
            if(i==border){
                border = maxPosition;
                steps++;
            }
        }
        return steps;
    }
}
```

```java
思路：为了求最小步数，*我们只有当不得不跳时，才跳*。
根据自己的理解，细化了下变量，及含义，希望对大家有帮助
解法思路依然是贪心，复杂度O(n)
    public int jump(int[] nums) {
        if (nums == null || nums.length <= 1) return 0;

        int currJumpMax = 0,  //以当前跳跃步数，能到的最远位置，比如: jump=1跳一次时，最远能到下标currJumpMax=2
            currPostMax = 0,  //当前位置能到的最远位置
            jump = 0,
            i = 0;

        while (i < nums.length - 1) { //不需要检查最后一个位置是因为，最后一个位置我们不用跳了已经
            currPostMax = Math.max(currPostMax, i + nums[i]);
            if (i == currJumpMax) { //已经走到了当前跳跃步数的边界，
                jump++; //我们不得不再跳一次
                currJumpMax = currPostMax; //并记录当前跳跃步数能到的最远位置
            }
            i++;
        }

        return jump;
    }
    ```