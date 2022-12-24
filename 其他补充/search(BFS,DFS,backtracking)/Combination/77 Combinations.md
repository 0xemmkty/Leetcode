# 77. Combinations

Given two integers n and k, return all possible combinations of k numbers chosen from the range [1, n].

You may return the answer in any order.

 
```
Example 1:

Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
```
题解参考：
https://leetcode.cn/problems/combinations/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-ma-/

## Solution 1 (基础代码，未剪枝)
```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        if (k <= 0 || n < k) {
            return res;
        }
        // 从 1 开始是题目的设定
        Deque<Integer> path = new ArrayDeque<>();
        dfs(n, k, 1, path, res);
        return res;
    }

    private void dfs(int n, int k, int begin, Deque<Integer> path, List<List<Integer>> res) {
        // 递归终止条件是：path 的长度等于 k
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }

        // 遍历可能的搜索起点
        for (int i = begin; i <= n; i++) {
            // 向路径变量里添加一个数
            path.addLast(i);
            // 下一轮搜索，设置的搜索起点要加 1，因为组合数理不允许出现重复的元素
            dfs(n, k, i + 1, path, res);
            // 重点理解这里：深度优先遍历有回头的过程，因此递归之前做了什么，递归之后需要做相同操作的逆向操作
            path.removeLast();
        }
    }
}

```

分析：
事实上，如果 n = 7, k = 4，从 55 开始搜索就已经没有意义了，这是因为：即使把 55 选上，后面的数只有 66 和 77，一共就 33 个候选数，凑不出 44 个数的组合。因此，搜索起点有上界，
可以归纳出：


搜索起点的上界 + 接下来要选择的元素个数 - 1 = n
其中，接下来要选择的元素个数 = k - path.size()，整理得到：


搜索起点的上界 = n - (k - path.size()) + 1
所以，我们的剪枝过程就是：把 i <= n 改成 i <= n - (k - path.size()) + 1 ：

## Solution 2 （剪枝）
```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        if (k <= 0 || n < k) {
            return res;
        }
        Deque<Integer> path = new ArrayDeque<>();
        dfs(n, k, 1, path, res);
        return res;
    }

    private void dfs(int n, int k, int index, Deque<Integer> path, List<List<Integer>> res) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }

        // 只有这里 i <= n - (k - path.size()) + 1 与参考代码 1 不同
        for (int i = index; i <= n - (k - path.size()) + 1; i++) {
            path.addLast(i);
            dfs(n, k, i + 1, path, res);
            path.removeLast();
        }
    }
}

```

## Solution 3(根据一个数选与不选)
按照每一个数选与不选画出二叉树
```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        if (k <= 0 || n < k) {
            return res;
        }

        // 为了防止底层动态数组扩容，初始化的时候传入最大长度
        Deque<Integer> path = new ArrayDeque<>(k);
        dfs(1, n, k, path, res);
        return res;
    }

    private void dfs(int begin, int n, int k, Deque<Integer> path, List<List<Integer>> res) {
        if (k == 0) {
            res.add(new ArrayList<>(path));
            return;
        }

        // 基础版本的递归终止条件：if (begin == n + 1) {
        // begin > n - k + 1 时停止搜索
        if (begin > n - k + 1) {
            return;
        }
        // 不选当前考虑的数 begin，直接递归到下一层
        dfs(begin + 1, n, k, path, res);

        // 不选当前考虑的数 begin，递归到下一层的时候 k - 1，这里 k 表示还需要选多少个数
        path.addLast(begin);
        dfs(begin + 1, n, k - 1, path, res);
        // 深度优先遍历有回头的过程，因此需要撤销选择
        path.removeLast();
    }
}

```