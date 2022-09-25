# 90. Subsets II

>Given an integer array nums that may contain duplicates, return all possible subsets (the power set).
The solution set **must not contain duplicate subsets**. Return the solution in any order.

## Solution 1(回溯)
简单易懂的版本;
```java
class Solution {
    private List<List<Integer>> ans;
    private List<Integer> path;

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        ans = new ArrayList<>();
        path = new ArrayList<>();
        // 首先排序，让相同的两个元素排到一起去，便于去重
        Arrays.sort(nums);
        int n = nums.length;
        // 使用 visited 数组来记录哪一个元素在当前路径中被使用了
        boolean[] visited = new boolean[n];
        // 开始回溯
        backtrace(nums, 0, visited, n);
        return ans;
    }

    private void backtrace(int[] nums, int start, boolean[] visited, int n) {
        // 首先加入当前路径
        ans.add(new ArrayList<>(path));
        // 从 start 开始遍历每一个元素，尝试加入路径中
        for (int i = start; i < n; ++i) {
            // 如果当前元素和前一个元素相同，而且前一个元素没有被访问，说明前一个相同的元素在当前层已经被用过了
            if (i > 0 && nums[i - 1] == nums[i] && !visited[i - 1]) continue;
            // 记录下来，用过了当前的元素
            visited[i] = true;
            path.add(nums[i]); // 放到路径中
            backtrace(nums, i + 1, visited, n); // 向下一个递归
            visited[i] = false; // 回溯
            path.remove(path.size() - 1);
        }
    }
}
```

简短版本：
```java
class Solution {
    private List<List<Integer>> ans;
    private List<Integer> path;

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        ans = new ArrayList<>();
        path = new ArrayList<>();
        // 首先排序，让相同的两个元素排到一起去，便于去重
        Arrays.sort(nums);
        int n = nums.length;
        // 使用 visited 数组来记录哪一个元素在当前路径中被使用了
        boolean[] visited = new boolean[n];
        // 开始回溯
        backtrace(nums, 0, visited, n);
        return ans;
    }

    private void backtrace(int[] nums, int start, boolean[] visited, int n) {
        // 首先加入当前路径
        ans.add(new ArrayList<>(path));
        // 从 start 开始遍历每一个元素，尝试加入路径中
        for (int i = start; i < n; ++i) {
            // 如果当前元素和前一个元素相同，而且前一个元素没有被访问，说明前一个相同的元素在当前层已经被用过了
            if (i > 0 && nums[i - 1] == nums[i] && !visited[i - 1]) continue;
            // 记录下来，用过了当前的元素
            visited[i] = true;
            path.add(nums[i]); // 放到路径中
            backtrace(nums, i + 1, visited, n); // 向下一个递归
            visited[i] = false; // 回溯
            path.remove(path.size() - 1);
        }
    }
}
```