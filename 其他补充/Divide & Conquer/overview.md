在计算机科学中，分治法是构建基于多项分支递归的一种很重要的算法范式。字面上的解释是「分而治之」，就是把一个复杂的问题分成两个或更多的相同或相似的子问题，直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并。

这个技巧是很多高效算法的基础，如排序算法（快速排序、归并排序）、傅立叶变换（快速傅立叶变换）。

https://leetcode.cn/tag/divide-and-conquer/problemset/

非常详细的复杂度分析：
https://juejin.cn/post/6993926231708139533

非常好的讲解（图和代码分析）：
https://juejin.cn/post/6989147825099522078

归并算法
```java
// 归并排序（Java-递归版）
static void merge_sort_recursive(int[] arr, int[] result, int start, int end) {
    if (start >= end)
        return;
    int len = end - start, mid = (len >> 1) + start;
    int start1 = start, end1 = mid;
    int start2 = mid + 1, end2 = end;
    merge_sort_recursive(arr, result, start1, end1);
    merge_sort_recursive(arr, result, start2, end2);
    int k = start;
    while (start1 <= end1 && start2 <= end2)
        result[k++] = arr[start1] < arr[start2] ? arr[start1++] : arr[start2++];
    while (start1 <= end1)
        result[k++] = arr[start1++];
    while (start2 <= end2)
        result[k++] = arr[start2++];
    for (k = start; k <= end; k++)
        arr[k] = result[k];
}

public static void merge_sort(int[] arr) {
    int len = arr.length;
    int[] result = new int[len];
    merge_sort_recursive(arr, result, 0, len - 1);
}
```



大概思路：（见第三个链接）
1.两个递归，二分法将一个数组不断二分最后得到单个数 （先分左，分完到右）
 第三个递归是merge
```java
public static int[] Merge_Sort(int[] arr, int start, int end) {
        //当start==end时，此时分组里只有一个元素，所以这是临界点
        if (start < end) {
            //分成左右两个分组，再进行递归
            int mid = (start + end) / 2;
            //左半边分组
            Merge_Sort(arr, start, mid);
            //右半边分组
            Merge_Sort(arr, mid + 1, end);
            //递归之后再归并归并一个大组
            Merge(arr, start, mid, end);
        }
        return arr;
    }
```

2. 归并算法，需要考虑：两段要合并数组的表示，两段数组有剩余元素，辅助数组覆盖原数组
i_start 和 i_end, j_start 和 j_end 分别为每次需要合并的数组，取两个数组中比较后的最小值放入额外空间，指针往后走
```java
import java.util.Arrays;

public class Merge_Sort {
    public static void main(String[] args) {
        int[] arr = {4,2,8,0,5,7,1,3,9};
        System.out.println(Arrays.toString(Merge_Sort(arr, 0, arr.length - 1)));
    }

    /**
     * @param arr   初始数组
     * @param start 开始分组
     * @param end   结束分组
     * @return
     */
    public static int[] Merge_Sort(int[] arr, int start, int end) {
        //当start==end时，此时分组里只有一个元素，所以这是临界点
        if (start < end) {
            //分成左右两个分组，再进行递归
            int mid = (start + end) / 2;
            //左半边分组
            Merge_Sort(arr, start, mid);
            //右半边分组
            Merge_Sort(arr, mid + 1, end);
            //递归之后再归并归并一个大组
            Merge(arr, start, mid, end);
        }
        return arr;
    }

    //归并方法
    public static void Merge(int[] arr, int start, int mid, int end) {
        //左边分组的起点i_start，终点i_end，也就是第一个有序序列
        int i_start = start;
        int i_end = mid;
        //右边分组的起点j_start，终点j_end，也就是第二个有序序列
        int j_start = mid + 1;
        int j_end = end;
        //额外空间初始化，数组长度为end-start+1
        int[] temp=new int[end-start+1];
        int len = 0;
        //合并两个有序序列
        while (i_start <= i_end && j_start <= j_end) {
            //当arr[i_start]<arr[j_start]值时，将较小元素放入额外空间，反之一样
            if (arr[i_start] < arr[j_start]) {
                temp[len] = arr[i_start];
                len++;
                i_start++;
            } else {
                temp[len] = arr[j_start];
                len++;
                j_start++;
            }
            //temp[len++]=arr[i_start]<arr[j_start]?arr[i_start++]:arr[j_start++];
        }

        //i这个序列还有剩余元素
        while(i_start<=i_end){
            temp[len] = arr[i_start];
            len++;
            i_start++;
        }
        //j这个序列还有剩余元素
        while(j_start<=j_end){
            temp[len] = arr[j_start];
            len++;
            j_start++;
        }
        //辅助空间数据覆盖到原空间
        for(int i=0;i<temp.length;i++){
            arr[start+i]=temp[i];
        }
    }
}
```