见 blind75 array 53

# 45. jump game II
（对比55，45是跳到最后一格，55是判断能否跳完）
You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

0 <= j <= nums[i] and
i + j < n
Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

## solution

方法一：动态规划
“动态规划”的两个步骤是思考“状态”以及“状态转移方程”。

有的资料又将“动态规划”分为 3 步：

base case：思考问题规模最小的时候，是什么情况；
update function：自下而上思考这个问题，即上面的“状态转移方程”；
gola：重点强调了输出是什么，很多时候输出并不一定是最后一个状态。
我觉得这种分法更细致一点，“状态”以及“状态转移方程”也没有问题，但是我觉得还要加上一个，思考一下输出是什么，即将第 2 种的第 3 步加上去，在下面的分析中，我还会强调这一点。

1、状态

题目中的关键字是“连续”，首先我们尝试就将状态定义成题目要求的结果，即：dp[i] 表示 nums 在区间 [0, i] 中连续子数组的最大和。在思考“状态转移方程”的时候，dp[i] 之前的，例如 dp[i - 1] 就有可能是是更前面的连续子数组的最大和，不利于我们分类讨论。

但是我们有相关的经验，例如「力扣」第 300 题：“最长上升子序列”，既然一个连续子数组一定要以一个数作为结尾，那么我们就将状态定义成：

dp[i]：表示以 nums[i] 结尾的连续子数组的最大和。

这里要注意一件事情：

如果状态的定义不是题目中的问题的定义，一定思考一下输出是什么，不能直接将最后一个状态返回回去。

输出应该是把所有的 dp[0]、dp[1]、……、dp[n - 1] 都看一遍，取最大值。 同样的情况也适用于「力扣」第 300 题：“最长上升子序列”。我经常在这一步“摔跟头”，请各位也留意。

2、状态转移方程

接下来分类讨论就变得容易多了，dp[i] 的值要看 dp[i - 1]，因为 nums[i] 一定被选取，那么 dp[i - 1] 的正负就作为分类的标准。

如果 dp[i - 1] >= 0，那么可以把 nums[i] 直接接在 dp[i - 1] 表示的那个数组的后面。

如果 dp[i - 1] < 0，那么加上前面的数反而我越来越小了，干脆“另起炉灶”吧，单独的一个 nums[i]，就是 dp[i]。

以上两种情况的最大值就是 dp[i] 的值，写出“状态转移方程 1”：

dp[i]={​dp[i−1]+nums[i],​nums[i],​​​ifdp[i−1]≥0​ifdp[i−1]<0​​

或者你还可以这样写，反正求的是最大值，我也不用分类讨论了，就这两种情况，直接取最大即可，因此还可以写出“状态转移方程 2”：

dp[i]=max{nums[i],dp[i−1]+nums[i]}

参考代码 1：根据“状态转移方程 1”

Python 代码：

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        size = len(nums)
        if size == 0:
            return 0
        dp = [0 for _ in range(size)]

        dp[0] = nums[0]
        for i in range(1, size):
            if dp[i - 1] >= 0:
                dp[i] = dp[i - 1] + nums[i]
            else:
                dp[i] = nums[i]
        return max(dp)
Java 代码：

public class Solution {

    public int maxSubArray(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        int[] dp = new int[len];
        dp[0] = nums[0];

        for (int i = 1; i < len; i++) {
            if (dp[i - 1] >= 0) {
                dp[i] = dp[i - 1] + nums[i];
            } else {
                dp[i] = nums[i];
            }
        }

        // 最后不要忘记全部看一遍求最大值
        int res = dp[0];
        for (int i = 1; i < len; i++) {
            res = Math.max(res, dp[i]);
        }
        return res;
    }

}
参考代码 2：根据“状态转移方程 2”

from typing import List


class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        size = len(nums)
        if size == 0:
            return 0
        dp = [0 for _ in range(size)]

        dp[0] = nums[0]
        for i in range(1, size):
            dp[i] = max(dp[i - 1] + nums[i], nums[i])
        return max(dp)
Java 代码：

public class Solution {

    public int maxSubArray(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        int[] dp = new int[len];
        dp[0] = nums[0];

        for (int i = 1; i < len; i++) {
            dp[i] = Math.max(nums[i], dp[i - 1] + nums[i]);
        }

        int res = dp[0];
        for (int i = 1; i < len; i++) {
            res = Math.max(res, dp[i]);
        }
        return res;
    }

}
复杂度分析：

时间复杂度：O(N)O(N)。
空间复杂度：O(N)O(N)。
参考代码 3：既然当前状态只与上一个状态有关，我们可以将空间复杂度压缩到 O(1)O(1)。

from typing import List


class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        size = len(nums)
        if size == 0:
            return 0
        # 起名叫 pre 表示的意思是“上一个状态”的值
        pre = nums[0]
        res = pre
        for i in range(1, size):
            pre = max(nums[i], pre + nums[i])
            res = max(res, pre)
        return res
Java 代码：

public class Solution {

    public int maxSubArray(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        // 起名叫 pre 表示的意思是“上一个状态”的值
        int pre = nums[0];
        int res = pre;
        for (int i = 1; i < len; i++) {
            pre = Math.max(nums[i], pre + nums[i]);
            res = Math.max(res, pre);
        }
        return res;
    }

}
复杂度分析：

时间复杂度：O(N)O(N)。
空间复杂度：O(1)O(1)。
#方法二：分治法
这个思路不是我想出来的，根据题目中的提示，在网络上搜索了一下，这是参考资料。

我“翻译”了一下，用自己认为比较易懂的编码方式写了出来。

我个人没有觉得这个方法很直接，并不像题目中说得那么好。

参考代码 4：

from typing import List


class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        size = len(nums)
        if size == 0:
            return 0
        return self.__max_sub_array(nums, 0, size - 1)

    def __max_sub_array(self, nums, left, right):
        if left == right:
            return nums[left]
        mid = (left + right) >> 1
        return max(self.__max_sub_array(nums, left, mid),
                   self.__max_sub_array(nums, mid + 1, right),
                   self.__max_cross_array(nums, left, mid, right))

    def __max_cross_array(self, nums, left, mid, right):
        # 一定包含 nums[mid] 元素的最大连续子数组的和，
        # 思路是看看左边"扩散到底"，得到一个最大数，右边"扩散到底"得到一个最大数
        # 然后再加上中间数
        left_sum_max = 0
        start_left = mid - 1
        s1 = 0
        while start_left >= left:
            s1 += nums[start_left]
            left_sum_max = max(left_sum_max, s1)
            start_left -= 1

        right_sum_max = 0
        start_right = mid + 1
        s2 = 0
        while start_right <= right:
            s2 += nums[start_right]
            right_sum_max = max(right_sum_max, s2)
            start_right += 1
        return left_sum_max + nums[mid] + right_sum_max
Java 代码：

public class Solution {

    public int maxSubArray(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        return maxSubArraySum(nums, 0, len - 1);
    }

    private int maxCrossingSum(int[] nums, int left, int mid, int right) {
        // 一定会包含 nums[mid] 这个元素
        int sum = 0;
        int leftSum = Integer.MIN_VALUE;
        // 左半边包含 nums[mid] 元素，最多可以到什么地方
        // 走到最边界，看看最值是什么
        // 计算以 mid 结尾的最大的子数组的和
        for (int i = mid; i >= left; i--) {
            sum += nums[i];
            if (sum > leftSum) {
                leftSum = sum;
            }
        }
        sum = 0;
        int rightSum = Integer.MIN_VALUE;
        // 右半边不包含 nums[mid] 元素，最多可以到什么地方
        // 计算以 mid+1 开始的最大的子数组的和
        for (int i = mid + 1; i <= right; i++) {
            sum += nums[i];
            if (sum > rightSum) {
                rightSum = sum;
            }
        }
        return leftSum + rightSum;

    }

    private int maxSubArraySum(int[] nums, int left, int right) {
        if (left == right) {
            return nums[left];
        }
        int mid = (left + right) >>> 1;
        return max3(maxSubArraySum(nums, left, mid),
                maxSubArraySum(nums, mid + 1, right),
                maxCrossingSum(nums, left, mid, right));
    }

    private int max3(int num1, int num2, int num3) {
        return Math.max(num1, Math.max(num2, num3));
    }
}
复杂度分析：

时间复杂度：O(N \log N)O(NlogN)，这里递归的深度是对数级别的，每一层需要遍历一遍数组（或者数组的一半、四分之一）。
空间复杂度：O(1)O(1)，仅需要常数个空间用于选取最大值。