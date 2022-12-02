# 410. Split Array Largest Sum

>Given an integer array nums and an integer k, split nums into k non-empty subarrays such that the largest sum of any subarray is minimized.

>Return the minimized largest sum of the split.

>A subarray is a contiguous part of the array.

## Solution 1 (binary search)
```java
public class Solution {

    public int splitArray(int[] nums, int m) {
        int max = 0;
        int sum = 0;

        // 计算「子数组各自的和的最大值」的上下界
        for (int num : nums) {
            max = Math.max(max, num);
            sum += num;
        }

        // 使用「二分查找」确定一个恰当的「子数组各自的和的最大值」，
        // 使得它对应的「子数组的分割数」恰好等于 m
        int left = max;
        int right = sum;
        while (left < right) {
            int mid = left + (right - left) / 2;

            int splits = split(nums, mid);
            if (splits > m) {
                // 如果分割数太多，说明「子数组各自的和的最大值」太小，此时需要将「子数组各自的和的最大值」调大
                // 下一轮搜索的区间是 [mid + 1, right]
                left = mid + 1;
            } else {
                // 下一轮搜索的区间是上一轮的反面区间 [left, mid]
                right = mid;
            }
        }
        return left;
    }

    /***
     *
     * @param nums 原始数组
     * @param maxIntervalSum 子数组各自的和的最大值
     * @return 满足不超过「子数组各自的和的最大值」的分割数
     */
    private int split(int[] nums, int maxIntervalSum) {
        // 至少是一个分割
        int splits = 1;
        // 当前区间的和
        int curIntervalSum = 0;
        for (int num : nums) {
            // 尝试加上当前遍历的这个数，如果加上去超过了「子数组各自的和的最大值」，就不加这个数，另起炉灶
            if (curIntervalSum + num > maxIntervalSum) {
                curIntervalSum = 0;
                splits++;
            }
            curIntervalSum += num;
        }
        return splits;
    }
}

```

时间复杂度：O(Nlog∑nums)，这里 N 表示输入数组的长度，∑nums 表示输入数组的和，代码在 [max(nums),∑nums] 区间里使用二分查找找到目标元素，而每一次判断分支需要遍历一遍数组，时间复杂度为 O(N)；
空间复杂度：O(1) ，只使用到常数个临时变量。

作者：liweiwei1419
链接：https://leetcode.cn/problems/split-array-largest-sum/solutions/242909/er-fen-cha-zhao-by-liweiwei1419-4/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## Solution 2 (dp)
时空复杂度较高，不推荐，仅做拓展思路用
```java
import java.util.Arrays;

public class Solution {

    public int splitArray(int[] nums, int m) {
        int len = nums.length;
        // 前缀和，preSum[i] = sum[0..i)
        int[] preSum = new int[len + 1];
        preSum[0] = 0;
        for (int i = 0; i < len; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
        }

        // 区间 [i..j] 的和 preSum(j + 1) - preSum(i)
        int[][] dp = new int[len][m + 1];
        // 初始化：由于要找最小值，初值赋值成为一个不可能达到的很大的值
        for (int i = 0; i < len; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }
        // 分割数为 1 ，即不分割的情况，所有的前缀和就是依次的状态值
        for (int i = 0; i < len; i++) {
            dp[i][1] = preSum[i + 1];
        }

        // 从分割数为 2 开始递推
        for (int k = 2; k <= m; k++) {
            // 还未计算出的 i 是从 j 的最小值的下一位开始，因此是 k - 1
            for (int i = k - 1; i < len; i++) {
                // j 表示第 k - 1 个区间的最后一个元素额下标，最小值为 k - 2，最大值为 len - 2（最后一个区间至少有 1 个元素）
                for (int j = k - 2; j < i; j++) {
                    dp[i][k] = Math.min(dp[i][k], Math.max(dp[j][k - 1], preSum[i + 1] - preSum[j + 1]));
                }
            }
        }
        return dp[len - 1][m];
    }
}



```
https://leetcode.cn/problems/split-array-largest-sum/solutions/242909/er-fen-cha-zhao-by-liweiwei1419-4/
(先欠着 哪天看)