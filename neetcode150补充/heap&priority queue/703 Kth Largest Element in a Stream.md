# 703. Kth Largest Element in a Stream

>Design a class to find the kth largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

>Implement KthLargest class:

>KthLargest(int k, int[] nums) Initializes the object with the integer k and the stream of integers nums.
int add(int val) Appends the integer val to the stream and returns the element representing the kth largest element in the stream.

## Solution 1
```
我在解题前想到heap，但是没有意识到优先队列，的自带排序
```

先说一个最暴力的解法：我们底层数据结构使用数组实现，当每次调用 add() 函数时，向数组中添加一个元素，然后调用 sort() 函数进行排序，返回排序后数组的第 KKK 个数字。该做法在每次调用 add() 函数时的时间复杂度为 O(K∗log(K))O(K*log(K))O(K∗log(K)) ，该时间复杂度太高，当 KKK 很大 / add()调用次数太多的时候，一定会超时。

从上面的分析中，我们已经看出来了，使用数组的核心问题是：数组自身不带排序功能，只能用 sort() 函数，导致时间复杂度过高。

因此我们考虑使用自带排序功能的数据结构——堆。


优先队列
优先队列自带排序
```
思路：用一个size是k的小顶堆来存贮前k个元素，堆顶是最小的元素，在循环数组的过程中，不断加入元素然后调整元素在堆中的位置，如果此时优先队列的大小大于 k，我们需要将优先队列的队头元素弹出，以保证优先队列的大小为 k，最后堆顶就是第K大元素的位置
复杂度分析：时间复杂度O(nlogk)，n是初始数组的大小，k是堆的大小，初始堆化和单次插入堆的复杂度都是O(logk)，数组的每个数都要插入堆中一次，所以是O(nlogk)。 空间复杂度：O(k), 即堆的大小
```
```java
class KthLargest {
    PriorityQueue<Integer> pq;
    int k;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        pq = new PriorityQueue<Integer>();
        for (int x : nums) {
            add(x);
        }
    }
    
    public int add(int val) {
        pq.offer(val);
        while (pq.size() > k) {
            pq.poll();
        }
        return pq.peek();
    }
}
```