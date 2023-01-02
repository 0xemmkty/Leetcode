# 53. Maximum Subarray

Given an integer array nums, find the 
subarray
 with the largest sum, and return its sum.

 
```
Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
```

## Solution 1 (dp)
```java
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

```

## Solution 2(分治)
```java
// 分治法: 线段树
class Solution {
    public int maxSubAray(int[] nums) {
        if (nums == null || nums.length <= 0)// 输入校验
            return 0;
        int len = nums.length;// 获取输入长度
        return getInfo(nums, 0, len - 1).mSum;
    }

    class wtevTree { //线段树
        int lSum;// 以左区间为端点的最大子段和
        int rSum;// 以右区间为端点的最大子段和
        int iSum;// 区间所有数的和
        int mSum;// 该区间的最大子段和
        wtevTree(int l, int r, int i, int m) { // 构造函数
            lSum = l;
            rSum = r;
            iSum = i;
            mSum = m;
        }
    }

    // 通过既有的属性,计算上一层的属性,一步步往上返回,获得线段树
    wtevTree pushUp(wtevTree leftT, wtevTree rightT) {
        // 新子段的lSum等于左区间的lSum或者左区间的 区间和 加上右区间的lSum
        int l = Math.max(leftT.lSum, leftT.iSum + rightT.lSum);
        // 新子段的rSum等于右区间的rSum或者右区间的 区间和 加上左区间的rSum
        int r = Math.max(leftT.rSum + rightT.iSum, rightT.rSum);
        // 新子段的区间和等于左右区间的区间和之和
        int i = leftT.iSum + rightT.iSum;
        // 新子段的最大子段和,其子段有可能穿过左右区间,或左区间,或右区间
        int m = Math.max(leftT.rSum + rightT.lSum, Math.max(leftT.mSum, rightT.mSum));
        return new wtevTree(l, r, i, m);
    }

    // 递归建立和获得输入区间所有子段的结构
    wtevTree getInfo(int[] nums, int left, int right) {
        if (left == right) // 若区间长度为1,其四个子段均为其值
            return new wtevTree(nums[left], nums[left], nums[left], nums[left]);
        int mid = (left + right) >> 1;// 获得中间点mid,右移一位相当于除以2,运算更快
        wtevTree leftT = getInfo(nums, left, mid);
        wtevTree rightT = getInfo(nums, mid + 1, right);//mid+1,左右区间没有交集。
        return pushUp(leftT, rightT);//递归结束后,做最后一次合并
    }
}
```

先欠着 有空来
```
这个分治方法类似于「线段树求解最长公共上升子序列问题」的 pushUp 操作。 也许读者还没有接触过线段树，没有关系，方法二的内容假设你没有任何线段树的基础。当然，如果读者有兴趣的话，推荐阅读线段树区间合并法解决多次询问的「区间最长连续上升序列问题」和「区间最大子段和问题」，还是非常有趣的。

我们定义一个操作 get(a, l, r) 表示查询 aa 序列 [l,r][l,r] 区间内的最大子段和，那么最终我们要求的答案就是 get(nums, 0, nums.size() - 1)。如何分治实现这个操作呢？对于一个区间 [l,r][l,r]，我们取 m = \lfloor \frac{l + r}{2} \rfloorm=⌊ 
2
l+r
​
 ⌋，对区间 [l,m][l,m] 和 [m+1,r][m+1,r] 分治求解。当递归逐层深入直到区间长度缩小为 11 的时候，递归「开始回升」。这个时候我们考虑如何通过 [l,m][l,m] 区间的信息和 [m+1,r][m+1,r] 区间的信息合并成区间 [l,r][l,r] 的信息。最关键的两个问题是：

我们要维护区间的哪些信息呢？
我们如何合并这些信息呢？
对于一个区间 [l,r][l,r]，我们可以维护四个量：

\textit{lSum}lSum 表示 [l,r][l,r] 内以 ll 为左端点的最大子段和
\textit{rSum}rSum 表示 [l,r][l,r] 内以 rr 为右端点的最大子段和
\textit{mSum}mSum 表示 [l,r][l,r] 内的最大子段和
\textit{iSum}iSum 表示 [l,r][l,r] 的区间和
以下简称 [l,m][l,m] 为 [l,r][l,r] 的「左子区间」，[m+1,r][m+1,r] 为 [l,r][l,r] 的「右子区间」。我们考虑如何维护这些量呢（如何通过左右子区间的信息合并得到 [l,r][l,r] 的信息）？对于长度为 11 的区间 [i, i][i,i]，四个量的值都和 \textit{nums}[i]nums[i] 相等。对于长度大于 11 的区间：

首先最好维护的是 \textit{iSum}iSum，区间 [l,r][l,r] 的 \textit{iSum}iSum 就等于「左子区间」的 \textit{iSum}iSum 加上「右子区间」的 \textit{iSum}iSum。
对于 [l,r][l,r] 的 \textit{lSum}lSum，存在两种可能，它要么等于「左子区间」的 \textit{lSum}lSum，要么等于「左子区间」的 \textit{iSum}iSum 加上「右子区间」的 \textit{lSum}lSum，二者取大。
对于 [l,r][l,r] 的 \textit{rSum}rSum，同理，它要么等于「右子区间」的 \textit{rSum}rSum，要么等于「右子区间」的 \textit{iSum}iSum 加上「左子区间」的 \textit{rSum}rSum，二者取大。
当计算好上面的三个量之后，就很好计算 [l,r][l,r] 的 \textit{mSum}mSum 了。我们可以考虑 [l,r][l,r] 的 \textit{mSum}mSum 对应的区间是否跨越 mm——它可能不跨越 mm，也就是说 [l,r][l,r] 的 \textit{mSum}mSum 可能是「左子区间」的 \textit{mSum}mSum 和 「右子区间」的 \textit{mSum}mSum 中的一个；它也可能跨越 mm，可能是「左子区间」的 \textit{rSum}rSum 和 「右子区间」的 \textit{lSum}lSum 求和。三者取大。

```

```java
class Solution {
    public class Status {
        public int lSum, rSum, mSum, iSum;

        public Status(int lSum, int rSum, int mSum, int iSum) {
            this.lSum = lSum;
            this.rSum = rSum;
            this.mSum = mSum;
            this.iSum = iSum;
        }
    }

    public int maxSubArray(int[] nums) {
        return getInfo(nums, 0, nums.length - 1).mSum;
    }

    public Status getInfo(int[] a, int l, int r) {
        if (l == r) {
            return new Status(a[l], a[l], a[l], a[l]);
        }
        int m = (l + r) >> 1;
        Status lSub = getInfo(a, l, m);
        Status rSub = getInfo(a, m + 1, r);
        return pushUp(lSub, rSub);
    }

    public Status pushUp(Status l, Status r) {
        int iSum = l.iSum + r.iSum;
        int lSum = Math.max(l.lSum, l.iSum + r.lSum);
        int rSum = Math.max(r.rSum, r.iSum + l.rSum);
        int mSum = Math.max(Math.max(l.mSum, r.mSum), l.rSum + r.lSum);
        return new Status(lSum, rSum, mSum, iSum);
    }
}
```


## Solution 3(贪心)
```java
class Solution{
    public int maxSubAray(int[] nums){
        //类似寻找最大最小值的题目,初始值一定要定义成理论上的最小最大值
        int result = Integer.MIN_VALUE;
        int numsSize = nums.length;
        int sum = 0;
        for (int i = 0; i < numsSize; i++){
            sum += nums[i];
            result = Math.max(result, sum);
            //如果sum < 0,重新开始找子序串(大概是，如果之前的和小于0，就丢弃之前的数列)
            if (sum < 0){
                sum = 0;
            }
        }

        return result;
    }
}
```