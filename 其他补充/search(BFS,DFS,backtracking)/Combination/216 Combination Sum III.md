# 216. Combination Sum III

Find all valid combinations of k numbers that sum up to n such that the following conditions are true:

Only numbers 1 through 9 are used.
Each number is used at most once.
Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

 ```
 Example 2:

Input: k = 3, n = 9
Output: [[1,2,6],[1,3,5],[2,3,4]]
Explanation:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.
```

## Solution 1 (backtracking)
重点： 其实当list.size()等于k的时候或者n<=0的时候就要终止递归
用n-i的值来做判断
```java
class Solution {
        public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(res, new ArrayList<>(), k, 1, n);
        return res;
    }

    private void dfs(List<List<Integer>> res, List<Integer> list, int k, int start, int n) {
        //终止条件，如果满足这个条件，再往下找也没什么意义了
        if (list.size() == k || n <= 0) {
            //如果找到一组合适的就把他加入到集合list中
            if (list.size() == k && n == 0)
                res.add(new ArrayList<>(list));
            return;
        }
        for (int i = start; i <= 9; i++) {
            //选择当前值
            list.add(i);
            //递归
            dfs(res, list, k, i + 1, n - i);
            //撤销选择
            list.remove(list.size() - 1);
        }
    }

}
```

## Solution 2 （递归解法，无回溯，仅供参考）
使用回溯是因为list是引用传递，当往回走的时候如果不把之前的值给删除，那么跳到下一个分支的时候就会把之前的值带到下一个分支，导致结果错误，如果list不是引用传递就不会有这个问题了，

```java
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(res, new ArrayList<>(), k, 1, n);
        return res;
    }

    private void dfs(List<List<Integer>> res, List<Integer> list, int k, int start, int n) {
        //终止条件，如果满足这个条件，再往下找也没什么意义了
        if (list.size() == k || n <= 0) {
            //如果找到一组合适的就把他加入到集合list中
            if (list.size() == k && n == 0)
                res.add(new ArrayList<>(list));
            return;
        }
        for (int i = start; i <= 9; i++) {
            //创建一个新的list，和原来的list撇清关系，对当前list的修改并不会影响到之前的list
            List<Integer> subList = new LinkedList<>(list);
            subList.add(i);
            //递归
            dfs(res, subList, k, i + 1, n - i);
            //注意这里没有撤销的操作，因为是在一个新的list中的修改，原来的list并没有修改，所以不需要撤销操作
        }
    }

```