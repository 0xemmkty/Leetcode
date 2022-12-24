# 47. Permutations II

Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

(different between 46 and 47 is, in lc46, the number is distinct, but in lc47, the number can be duplicates.)

## Solution 1 (回溯)
```
代码实现方面，在第 46 题的基础上，要加上这样一段代码：

Java

if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
    continue;
}
// 因为两个值相等，且前一个被用过的时候，必定产生重复
```
```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> permuteUnique(int[] nums) {
        int len = nums.length;
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) {
            return res;
        }

        // 排序（升序或者降序都可以），排序是剪枝的前提
        Arrays.sort(nums);

        boolean[] used = new boolean[len];
        // 使用 Deque 是 Java 官方 Stack 类的建议
        Deque<Integer> path = new ArrayDeque<>(len);
        dfs(nums, len, 0, used, path, res);
        return res;
    }

    private void dfs(int[] nums, int len, int depth, boolean[] used, Deque<Integer> path, List<List<Integer>> res) {
        if (depth == len) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < len; ++i) {
            if (used[i]) {
                continue;
            }

            // 剪枝条件：i > 0 是为了保证 nums[i - 1] 有意义
            // 写 !used[i - 1] 是因为 nums[i - 1] 在深度优先遍历的过程中刚刚被撤销选择
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue;
            }

            path.addLast(nums[i]);
            used[i] = true;

            dfs(nums, len, depth + 1, used, path, res);
            // 回溯部分的代码，和 dfs 之前的代码是对称的
            used[i] = false;
            path.removeLast();
        }
    }
}
```
```
复杂度分析：（理由同第 46 题，重复元素越多，剪枝越多。但是计算复杂度的时候需要考虑最差情况。）

时间复杂度：O(N×N!)，这里 N 为数组的长度。
空间复杂度：O(N×N!)。

作者：liweiwei1419
链接：https://leetcode.cn/problems/permutations-ii/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liwe-2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

 