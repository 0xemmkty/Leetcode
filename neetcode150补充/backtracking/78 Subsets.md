# 78. Subsets

>Given an integer array nums of unique elements, return all possible subsets (the power set).

>The solution set must not contain duplicate subsets. Return the solution in any order.

## Solution 1 (dfs，回溯)
```java
class Solution {
    List<Integer> t = new ArrayList<Integer>();
    List<List<Integer>> ans = new ArrayList<List<Integer>>();

    public List<List<Integer>> subsets(int[] nums) {
        dfs(0, nums);
        return ans;
    }

    public void dfs(int cur, int[] nums) {
        if (cur == nums.length) {
            ans.add(new ArrayList<Integer>(t));
            return;
        }
        t.add(nums[cur]);
        dfs(cur + 1, nums);
        t.remove(t.size() - 1);
        dfs(cur + 1, nums);
    }
}
```
```
复杂度分析

时间复杂度：O(n×2^n )。一共 2^n个状态，每种状态需要 O(n) 的时间来构造子集。

空间复杂度：O(n)。临时数组 t 的空间代价是 O(n)，递归时栈空间的代价为 O(n)。

```

## Solution 2 (迭代)
```java
class Solution {
    List<Integer> t = new ArrayList<Integer>();
    List<List<Integer>> ans = new ArrayList<List<Integer>>();

    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        for (int mask = 0; mask < (1 << n); ++mask) {
            t.clear();
            for (int i = 0; i < n; ++i) {
                if ((mask & (1 << i)) != 0) {
                    t.add(nums[i]);
                }
            }
            ans.add(new ArrayList<Integer>(t));
        }
        return ans;
    }
}
```
```
复杂度分析

时间复杂度：O(n×2^n )。一共 2^n个状态，每种状态需要 O(n) 的时间来构造子集。

空间复杂度：O(n)。即构造子集使用的临时数组 t 的空间代价。





