# 31. Next Permutation

A permutation of an array of integers is an arrangement of its members into a sequence or linear order.

For example, for arr = [1,2,3], the following are all the permutations of arr: [1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1].
The next permutation of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the next permutation of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

For example, the next permutation of arr = [1,2,3] is [1,3,2].
Similarly, the next permutation of arr = [2,3,1] is [3,1,2].
While the next permutation of arr = [3,2,1] is [1,2,3] because [3,2,1] does not have a lexicographical larger rearrangement.
Given an array of integers nums, find the next permutation of nums.

The replacement must be in place and use only constant extra memory.

乱入：不是特别看得懂题，附上可视化链接：
https://leetcode.cn/problems/next-permutation/solution/xia-yi-ge-pai-lie-suan-fa-xiang-jie-si-lu-tui-dao-/
 ## Solution 1
 ```
 算法过程
标准的 “下一个排列” 算法可以描述为：

从后向前 查找第一个 相邻升序 的元素对 (i,j)，满足 A[i] < A[j]。此时 [j,end) 必然是降序
在 [j,end) 从后向前 查找第一个满足 A[i] < A[k] 的 k。A[i]、A[k] 分别就是上文所说的「小数」、「大数」
将 A[i] 与 A[k] 交换
可以断定这时 [j,end) 必然是降序，逆置 [j,end)，使其升序
如果在步骤 1 找不到符合的相邻元素对，说明当前 [begin,end) 为一个降序顺序，则直接跳到步骤 4

作者：imageslr
链接：https://leetcode.cn/problems/next-permutation/solution/xia-yi-ge-pai-lie-suan-fa-xiang-jie-si-lu-tui-dao-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
```
首先从后向前查找第一个相邻升序的元素对 (i,j)。
然后在 [j,end) 从后向前查找第一个大于 A[i] 的值 A[k]。
将 A[i] 与 A[k] 交换。
这时 [j,end) 必然是降序，逆置 [j,end)，使其升序。
```
 ```java
 class Solution {
    public void nextPermutation(int[] nums) {
        int len = nums.length;
        if(len<=1) return;

        for(int i=len-1;i>=1;i--){
            if(nums[i]>nums[i-1]){            //找到相邻升序
                for(int j=len-1;j>=i;j--){
                    if(nums[j]>nums[i-1]){    //找到最右边大于nums[i-1]的数，并交换
                        int tmp = nums[i-1];
                        nums[i-1] = nums[j];
                        nums[j] = tmp;
                        break;
                    }
                }
                Arrays.sort(nums,i,len);      //将后面降序变为升序
                return;
            }
        }
        Arrays.sort(nums);                     
    }
}
```