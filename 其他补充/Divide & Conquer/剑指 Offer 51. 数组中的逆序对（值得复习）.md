# 剑指 Offer 51. 数组中的逆序对 （值得复习）
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

 

示例 1:

输入: [7,5,6,4]
输出: 5

## Solution 1（归并排序）
(大概懂，但值得反复学习)
归并排序 分和治过程：
分：
[7, 3, 2, 6, 0, 1, 5, 4]
[7,3,2,6]   [0,1,5,4]
[7,3]   [2,6]   [0,1]   [5,4]
[7] [3] [2] [6] [0] [1] [5] [4]

并（治）：
[3,7]   [2,6]   [0,1]   [4,5]
[2,3,6,7]   [0,1,4,5]
[0,1,2,3,4,5,6,7]

>合并阶段 本质上是 合并两个排序数组 的过程，而每当遇到 **左子数组当前元素 > 右子数组当前元素** 时，意味着 「左子数组当前元素 至 末尾元素」 与 「右子数组当前元素」 构成了若干 「逆序对」 。

如下图所示，为左子数组 [2, 3, 6, 7]与 右子数组 [0, 1, 4, 5]的合并与逆序对统计过程。
即 2>0时，2，3，6，7 都 > 0       逆数对+4
   2>1时，同理                    逆数对+4
   2<4时，2向后一位，到3. 3也<4, 向后移一位，到6
   6>4,                            逆数对+2
   6>5,                             逆数对+2

```java
class Solution {
    //利用归并排序解答，在合并的时候，当左边的大于右边，就计算逆序数。
    //计算公式； mid-left+1
    //定义一个全局的计数器变量
    int count = 0;
    public int reversePairs(int[] nums) {
        this.count = 0;
        mergeSort(nums, 0, nums.length-1);
        return count;
    }
    public void mergeSort(int[] nums,int left,int right){
        //当只有一个节点的时候，直接返回，退出递归
        if(left >= right){
            return;
        }
        int mid = (left+right)/2;
        //左拆分
        mergeSort(nums,left,mid);
        //右拆分
        mergeSort(nums,mid+1,right);
        //合并
        merge(nums,left,mid,right);
    }
    public void merge(int[] nums,int left,int mid,int right){
        //定义一个临时数组
        int[] temp = new int[right-left+1];
        //定义一个指针，指向第一个数组的第一个元素
        int i = left;
        //定义一个指针，指向第二个数组的第一个元素
        int j = mid+1;
        //定义一个指针，指向临时数组的第一个元素
        int t = 0;
        //当两个数组都有元素的时候，遍历比较每个元素大小
        while(i <= mid && j <= right){
            //比较两个数组的元素，取较小的元素加入到，临时数组中
            //并将两个指针指向下一个元素
            if(nums[i] <= nums[j]){
                temp[t++] = nums[i++];
            }else{
                //当左边数组的大与右边数组的元素时，就对当前元素以及后面的元素的个数进行统计，
                //此时这个数就是，逆序数
                //定义一个计数器，记下每次合并中存在的逆序数。
                count += mid-i+1;
                temp[t++] = nums[j++];
            }
        }
        //当左边的数组没有遍历完成后，直接将剩余元素加入到临时数组中
        while(i <= mid){
            temp[t++] = nums[i++];
        }
        //当右边的数组没有遍历完成后，直接将剩余元素加入到临时数组中
        while(j <= right){
            temp[t++] =nums[j++];
        }
        //将新数组中的元素，覆盖nums旧数组中的元素。
        //此时数组的元素已经是有序的
        for(int k =0; k< temp.length;k++){
            nums[left+k] = temp[k];
        }
    }
}
```


```java
  int count;
    public int reversePairs(int[] nums) {
        this.count = 0;
        merge(nums, 0, nums.length - 1);
        return count;
    }

    public void merge(int[] nums, int left, int right) {
        int mid = left + ((right - left) >> 1);
        if (left < right) {
            merge(nums, left, mid);
            merge(nums, mid + 1, right);
            mergeSort(nums, left, mid, right);
        }
    }

    public void mergeSort(int[] nums, int left, int mid, int right) {
        int[] temparr = new int[right - left + 1];
        int index = 0;
        int temp1 = left, temp2 = mid + 1;

        while (temp1 <= mid && temp2 <= right) {
            if (nums[temp1] <= nums[temp2]) {
                temparr[index++] = nums[temp1++];
            } else {
                //用来统计逆序对的个数
                count += (mid - temp1 + 1);
                temparr[index++] = nums[temp2++];
            }
        }
        //把左边剩余的数移入数组
        while (temp1 <= mid) {
            temparr[index++] = nums[temp1++];
        }
        //把右边剩余的数移入数组
        while (temp2 <= right) {
            temparr[index++] = nums[temp2++];
        }
        //把新数组中的数覆盖nums数组
        for (int k = 0; k < temparr.length; k++) {
            nums[k + left] = temparr[k];
        }
    }
    ```

```java
class Solution {
    int[] nums, tmp;
    public int reversePairs(int[] nums) {
        this.nums = nums;
        tmp = new int[nums.length];
        return mergeSort(0, nums.length - 1);
    }
    private int mergeSort(int l, int r) {
        // 终止条件
        if (l >= r) return 0;
        // 递归划分
        int m = (l + r) / 2;
        int res = mergeSort(l, m) + mergeSort(m + 1, r);
        // 合并阶段
        int i = l, j = m + 1;
        for (int k = l; k <= r; k++)
            tmp[k] = nums[k];
        for (int k = l; k <= r; k++) {
            if (i == m + 1)
                nums[k] = tmp[j++];
            else if (j == r + 1 || tmp[i] <= tmp[j])
                nums[k] = tmp[i++];
            else {
                nums[k] = tmp[j++];
                res += m - i + 1; // 统计逆序对
            }
        }
        return res;
    }
}

```
复杂度分析：
时间复杂度 O(NlogN) ： 其中 N 为数组长度；归并排序使用 O(NlogN) 时间；
空间复杂度 O(N) ： 辅助数组 tmp 占用 O(N) 大小的额外空间；

## SOlution 2 (树状结构)
仅作科普，即知道有这么一个东西，不需要掌握（？
```java
import java.util.HashMap;
import java.util.Map;
import java.util.Set;
import java.util.TreeSet;

public class Solution {

    public int reversePairs(int[] nums) {
        int len = nums.length;

        if (len < 2) {
            return 0;
        }

        // 离散化：使得数字更紧凑，节约树状数组的空间
        // 1、使用二分搜索树是为了去掉重复元素
        Set<Integer> treeSet = new TreeSet<>();
        for (int i = 0; i < len; i++) {
            treeSet.add(nums[i]);
        }

        // 2、把排名存在哈希表里方便查询
        Map<Integer, Integer> rankMap = new HashMap<>();
        int rankIndex = 1;
        for (Integer num : treeSet) {
            rankMap.put(num, rankIndex);
            rankIndex++;
        }

        int count = 0;
        // 在树状数组内部完成前缀和的计算
        // 规则是：从后向前，先给对应的排名 + 1，再查询前缀和
        FenwickTree fenwickTree = new FenwickTree(rankMap.size());

        for (int i = len - 1; i >= 0; i--) {
            int rank = rankMap.get(nums[i]);
            fenwickTree.update(rank, 1);
            count += fenwickTree.query(rank - 1);
        }
        return count;
    }

    private class FenwickTree {
        private int[] tree;
        private int len;

        public FenwickTree(int n) {
            this.len = n;
            tree = new int[n + 1];
        }

        /**
         * 单点更新：将 index 这个位置 + delta
         *
         * @param i
         * @param delta
         */
        public void update(int i, int delta) {
            // 从下到上，最多到 size，可以等于 size
            while (i <= this.len) {
                tree[i] += delta;
                i += lowbit(i);
            }
        }


        // 区间查询：查询小于等于 tree[index] 的元素个数
        // 查询的语义是「前缀和」
        public int query(int i) {
            // 从右到左查询
            int sum = 0;
            while (i > 0) {
                sum += tree[i];
                i -= lowbit(i);
            }
            return sum;
        }

        public int lowbit(int x) {
            return x & (-x);
        }
    }
}

作者：liweiwei1419
链接：https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/bao-li-jie-fa-fen-zhi-si-xiang-shu-zhuang-shu-zu-b/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```