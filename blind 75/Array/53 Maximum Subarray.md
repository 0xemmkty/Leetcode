#53. Maximum Subarray

>Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
A subarray is a contiguous part of an array.

##Solution 1(贪心)

2ms
时间复杂度O(n),空间复杂度O(1)

    class Solution {
    public int maxSubArray(int[] nums) {
    /*
     * 	题目要求我们只求最优解，即获取最大的值，那么很容易会考虑到贪心算法
     * 	我们需要考虑到贪心抛弃，当你的tmp值加到负值(为0其实也可以抛弃，因为没有用处)的时候，那么前面的子串和后面的字符组合只会造成负面影响(贪心负影响,通俗的说就是前面子串和后面组合还不如后面本身大)，
     * 	因此，我们贪心地舍弃掉前面的子串，重新建立子串找最大值
     * */
    	int maxans = nums[0];
    	//保存临时值
    	int tmp = 0;
    	for(int num : nums) {
    		tmp += num;
    		if(tmp > maxans) maxans = tmp;
    		if(tmp < 0) tmp = 0;
    	}
    	return maxans;
    }
};

##Solution2 动态规划
时间复杂度O(n),空间复杂度O(1)

    public class Solution {

    public int maxSubArray(int[] nums) {
        int pre = 0;
        int res = nums[0];
        for (int num : nums) {
            pre = Math.max(pre + num, num);
            res = Math.max(res, pre);
        }
        return res;
    }
    }

    //dp[i]={ 
    dp[i−1]+nums[i], ifdp[i−1]>0
    nums[i], ifdp[i−1]≤0
    }
​
  
##Solution 3(动态规划) 不优化空间版本

    public class Solution {

    public int maxSubArray(int[] nums) {
        int len = nums.length;
        // dp[i] 表示：以 nums[i] 结尾的连续子数组的最大和
        int[] dp = new int[len];
        dp[0] = nums[0];

        for (int i = 1; i < len; i++) {
            if (dp[i - 1] > 0) {
                dp[i] = dp[i - 1] + nums[i];
            } else {
                dp[i] = nums[i];
            }
        }

        // 也可以在上面遍历的同时求出 res 的最大值，这里我们为了语义清晰分开写，大家可以自行选择
        int res = dp[0];
        for (int i = 1; i < len; i++) {
            res = Math.max(res, dp[i]);
        }
        return res;
    }
 }

作者：liweiwei1419
链接：https://leetcode.cn/problems/maximum-subarray/solution/dong-tai-gui-hua-fen-zhi-fa-python-dai-ma-java-dai/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

​


