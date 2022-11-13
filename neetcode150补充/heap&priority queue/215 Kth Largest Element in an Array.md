# 215. Kth Largest Element in an Array

>Given an integer array nums and an integer k, return the kth largest element in the array.

>Note that it is the kth largest element in the sorted order, not the kth distinct element.

>You must solve it in O(n) time complexity.

## Solution 1(暴力解法)
```java
 import java.util.Arrays;

public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        Arrays.sort(nums);
        return nums[len - k];
    }
}
```
时间复杂度：O(NlogN)，这里 N 是数组的长度，算法的性能消耗主要在排序，JDK 默认使用快速排序，因此时间复杂度为 O(NlogN)；
空间复杂度：O(logN)，这里认为编程语言使用的排序方法是「快速排序」，空间复杂度为递归调用栈的高度，为 logN

## Solution 2 (pq)
```java
import java.util.Comparator;
import java.util.PriorityQueue;

public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        // 使用一个含有 k 个元素的最小堆，PriorityQueue 底层是动态数组，为了防止数组扩容产生消耗，可以先指定数组的长度
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k, Comparator.comparingInt(a -> a));
        // Java 里没有 heapify ，因此我们逐个将前 k 个元素添加到 minHeap 里
        for (int i = 0; i < k; i++) {
            minHeap.offer(nums[i]);
        }

        for (int i = k; i < len; i++) {
            // 看一眼，不拿出，因为有可能没有必要替换
            Integer topElement = minHeap.peek();
            // 只要当前遍历的元素比堆顶元素大，堆顶弹出，遍历的元素进去
            if (nums[i] > topElement) {
                // Java 没有 replace()，所以得先 poll() 出来，然后再放回去
                minHeap.poll();
                minHeap.offer(nums[i]);
            }
        }
        return minHeap.peek();
    }
}

```
复杂度分析：

时间复杂度：O(NlogK)，遍历数据 O(N)，堆内元素调整 O(logK)；
空间复杂度：O(K)。

pq一个循环解决：
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        for (int num : nums) {
            heap.add(num);
            if (heap.size() > k) {
                heap.poll();
            }
        }
        return heap.peek();
    }
}
```

## Solution 3 (quick sort)
```java
import java.util.Random;


class Solution {
    
    private final static Random random = new Random(System.currentTimeMillis());
    
    public int findKthLargest(int[] nums, int k) {
        // 第 1 大的数，下标是 len - 1;
        // 第 2 大的数，下标是 len - 2;
        // ...
        // 第 k 大的数，下标是 len - k;
        int len = nums.length;
        int target = len - k;
        
        int left = 0;
        int right = len - 1;
        
        while (true) {
            int pivotIndex = partition(nums, left, right);
            if (pivotIndex == target) {
                return nums[pivotIndex]; 
            } else if (pivotIndex < target) {
                left = pivotIndex + 1; 
            } else {
                // pivotIndex > target
                right = pivotIndex - 1; 
            }
        }
    }
    
    private int partition(int[] nums, int left, int right) {
        int randomIndex = left + random.nextInt(right - left + 1);
        swap(nums, left, randomIndex);
        
        
        // all in nums[left + 1..le) <= pivot;
        // all in nums(ge..right] >= pivot;
        int pivot = nums[left];
        int le = left + 1;
        int ge = right;
        
        while (true) {
            while (le <= ge && nums[le] < pivot) {
                le++;
            }
            
            while (le <= ge && nums[ge] > pivot) {
                ge--;
            }
            
            if (le >= ge) {
                break;
            }
            swap (nums, le, ge);
            le++;
            ge--;
        }
        
        swap(nums, left, ge);
        return ge;
    }
    
    private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
    }
    
}


```
时间复杂度：O(N)O(N)O(N)，这里 NNN 是数组的长度，理由可以参考本题解下用户 @ZLW 的评论，需要使用主定理进行分析；
空间复杂度：O(1)O(1)O(1)，在我的代码里没有使用递归，在逐渐缩小搜索区间的过程中只使用到常数个变量。


