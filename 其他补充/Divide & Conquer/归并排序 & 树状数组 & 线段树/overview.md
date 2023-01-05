一个总结，不过用的是python
https://leetcode.cn/problems/count-of-smaller-numbers-after-self/solution/4chong-jie-fa-yi-wang-da-jin-pai-xu-shu-5vvds/

树状数组 https://leetcode.cn/problems/count-of-smaller-numbers-after-self/solution/shu-zhuang-shu-zu-by-liweiwei1419/


java模板-归并排序
```java
  //归并排序入口
    public void mergeSort(int[] nums){
        MergeSort(nums,0,nums.length-1);
    }

    /**
     * 归并排序
     * @param nums 待排序数组
     * @param start 数组开始的下标
     * @param end 数组结束的下标
     */
    private void MergeSort(int[] nums,int start,int end){
        if(start<end){
            int mid=start+(end-start)/2;
            MergeSort(nums,start,mid); //将无序数组划分
            MergeSort(nums,mid+1,end); //将无序数组划分
            merge(nums,start,mid,end); //再将两个有序数组合并
        }
    }

    /**
     *  双指针合并两个有序数组
     * @param nums
     * @param start
     * @param mid
     * @param end
     */
    private void merge(int[]nums, int start, int mid, int end){
        int P1=start;
        int P2=mid+1;
        int tmp[]=new int[end-start+1]; //需要借助额外的O(n)空间来存储合并后的数组
        int cur=0;
        while (P1<=mid&&P2<=end){
            if(nums[P1]<nums[P2]){
               tmp[cur]=nums[P1];
               P1++;
            }else {
               tmp[cur]=nums[P2];
               P2++;
            }
            cur++;
        }
        while (P1<=mid){
            tmp[cur]=nums[P1];
            P1++;
            cur++;
        }
        while (P2<=end){
            tmp[cur]=nums[P2];
            P2++;
            cur++;
        }
        for (int i = 0; i < res.length ; i++) {
             nums[i+start]=tmp[i];
        }
    }

```

树状数组原理详解：

首先我们需要知道lowbit(x)是返回x的最后一个1，例如二进制数10100就返回100.
树状数组模板
```java
// 上来先把三个方法写出来
{
    int[] tree;
    int lowbit(int x) {
        return x & -x;
    }
    // 查询前缀和的方法
    int query(int x) {
        int ans = 0;
        for (int i = x; i > 0; i -= lowbit(i)) ans += tree[i];
        return ans;
    }
    // 在树状数组 x 位置中增加值 u
    void add(int x, int u) {
        for (int i = x; i <= n; i += lowbit(i)) tree[i] += u;
    }
}

// 初始化「树状数组」，要默认数组是从 1 开始
{
    for (int i = 0; i < n; i++) add(i + 1, nums[i]);
}

// 使用「树状数组」：
{   
    void update(int i, int val) {
        // 原有的值是 nums[i]，要使得修改为 val，需要增加 val - nums[i]
        add(i + 1, val - nums[i]); 
        nums[i] = val;
    }
    
    int sumRange(int l, int r) {
        return query(r + 1) - query(l);
    }
}
```