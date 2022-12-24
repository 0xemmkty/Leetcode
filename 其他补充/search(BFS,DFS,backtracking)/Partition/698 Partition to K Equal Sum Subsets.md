# 698. Partition to K Equal Sum Subsets

Given an integer array nums and an integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are all equal.

```
Example 1:

Input: nums = [4,3,2,3,5,2,1], k = 4
Output: true
Explanation: It is possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.
```
## Solution 1 （回溯，球视角--这个球进哪个桶）
https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/solution/by-lfool-d9o7/


已优化版本 (建议看网址学习优化过程)
```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
    // 确立target的值
    int sum = 0;
    for (int i = 0; i < nums.length; i++) sum += nums[i];
    if (sum % k != 0) return false;
    int target = sum / k;
    // 优化一：排序优化
    Arrays.sort(nums);
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int temp = nums[l];
        nums[l] = nums[r];
        nums[r] = temp;
        l++;
        r--;
    }
    return backtrack(nums, 0, new int[k], k, target);
}
private boolean backtrack(int[] nums, int index, int[] bucket, int k, int target) {
    // 结束条件优化
    if (index == nums.length) return true;
    for (int i = 0; i < k; i++) {
        // 优化点二：如果当前等于下一个，结果是一样的，所以不考虑这种情况，直接continue
        if (i > 0 && bucket[i] == bucket[i - 1]) continue;
        // 剪枝， 如果加起来值超过了target（有先前的排序，so好判断） continue
        if (bucket[i] + nums[index] > target) continue;
        bucket[i] += nums[index];
        if (backtrack(nums, index + 1, bucket, k, target)) return true;
        bucket[i] -= nums[index];
    }
    return false;
}
}
```

## Solution 2 （回溯，桶视角——这个桶收哪个球）
 简略看看，主要是看网址的优化过程和分析过程
```java
// 备忘录，存储 used 的状态
private HashMap<Integer, Boolean> memo = new HashMap<>();

public boolean canPartitionKSubsets(int[] nums, int k) {
    int sum = 0;
    for (int i = 0; i < nums.length; i++) sum += nums[i];
    if (sum % k != 0) return false;
    int target = sum / k;
    // 使用位图技巧
    int used = 0;
    int[] bucket = new int[k + 1];
    return backtrack(nums, 0, bucket, k, target, used);
}
private boolean backtrack(int[] nums, int start, int[] bucket, int k, int target, int used) {
    // k 个桶均装满
    if (k == 0) return true;

    // 当前桶装满了，开始装下一个桶
    if (bucket[k] == target) {
        // 注意：桶从下一个开始；球从第一个开始
        boolean res = backtrack(nums, 0, bucket, k - 1, target, used);
        memo.put(used, res);
        return res;
    }

    if (memo.containsKey(used)) {
        // 如果当前状态曾今计算过，就直接返回，不要再递归穷举了
        return memo.get(used);
    }

    // 第 k 个桶开始对每一个球选择进行选择是否装入
    for (int i = start; i < nums.length; i++) {
        // 如果当前球已经被装入，则跳过
        if (((used >> i) & 1) == 1) continue;
        // 如果装入当前球，桶溢出，则跳过
        if (bucket[k] + nums[i] > target) continue;

        // 装入 && 标记已使用
        bucket[k] += nums[i];
        // 将第 i 位标记为 1
        used |= 1 << i;

        // 开始判断是否选择下一个球
        // 注意：桶依旧是当前桶；球是下一个球
        // 注意：是 i + 1
        if (backtrack(nums, i + 1, bucket, k, target, used)) return true;

        // 拿出 && 标记未使用
        bucket[k] -= nums[i];
        // 将第 i 位标记为 0
        used ^= 1 << i;
    }
    // 如果所有球均不能使所有桶刚好装满
    return false;
}

作者：lfool
链接：https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/solution/by-lfool-d9o7/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```