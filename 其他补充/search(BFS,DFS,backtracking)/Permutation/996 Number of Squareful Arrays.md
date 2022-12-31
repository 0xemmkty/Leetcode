# 996. Number of Squareful Arrays

An array is squareful if the sum of every pair of adjacent elements is a perfect square.

Given an integer array nums, return the number of permutations of nums that are squareful.

Two permutations perm1 and perm2 are different if there is some index i such that perm1[i] != perm2[i].

 
```
Example 1:

Input: nums = [1,17,8]
Output: 2
Explanation: [1,8,17] and [17,8,1] are the valid permutations.
Example 2:

Input: nums = [2,2,2]
Output: 1
```

## Solution 1(backtracking)
解题思路
剪枝1：如果当前位的数使用，则跳过

if(visited[i] == true)
剪枝2：（参考47. 全排列 II）

if(i>0 && nums[i] == nums[i-1] && !visited[i-1])
剪枝3（重点）：如果当前路径不为空，那么判断即将要加入的nums[i]与track.getLast()，这两个连续的相邻之和是否为一个完全平方数。如果不是完全平方数，那么该路径不可行，跳过该路径（剪枝）。

if(!track.isEmpty() && !isSquare(track.getLast(), nums[i]))

作者：lixianfeng
链接：https://leetcode.cn/problems/number-of-squareful-arrays/solution/javahui-su-jian-zhi-yun-xing-shi-jian-1ms-by-lixia/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```java
class Solution {
    int res = 0;
public int numSquarefulPerms(int[] A) {
    // 排序是剪枝2的前提
    Arrays.sort(A);
    LinkedList<Integer> track = new LinkedList<>();
    boolean[] visited = new boolean[A.length];
    backtrack(A, visited, track);
    return res;
}
public void backtrack(int[] nums, boolean[] visited, LinkedList<Integer> track){
    if(track.size() == nums.length){
        res++;
        return;
    }
    for(int i=0; i<nums.length; i++){
        // 剪枝1
        if(visited[i]){
            continue;
        }
        // 剪枝2
        if(i>0 && nums[i] == nums[i-1] && !visited[i-1]){
            continue;
        }
        // 剪枝3
        if(!track.isEmpty() && !isSquare(track.getLast(), nums[i])){
            continue;
        }
        track.add(nums[i]);
        visited[i] = true;
        backtrack(nums, visited, track);
        visited[i] = false;
        track.pollLast();
    }
}
// 判断a+b之和是不是一个完全平方数
public boolean isSquare(int a, int b){
    int tmp = a + b;
    int sq = (int)Math.sqrt(tmp);
    return sq * sq == tmp;
}

}
```