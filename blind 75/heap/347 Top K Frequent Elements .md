# 347. Top K Frequent Elements

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

```
Example 1:
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

## Solution 1
这道题是典型的TopK问题，数据结构采用小顶堆最方便，堆中存储k个，刚好就是频率前k高的元素。

先遍历数组，利用HashMap存储每个元素出现的频率
创建一个小顶堆，用来存放前k个频率的元素
遍历map集合，如果堆中的元素少于k个，那就直接将元素放入堆中，如果堆中的元素大于k个，就判断堆顶元素和当前元素x的大小（小顶堆的堆顶是堆中最小的元素）


```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++) {
            map.put(nums[i],map.getOrDefault(nums[i],0)+1);//把出现的频率存入map中
        }
        
        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>() {
            public int compare(Integer a,Integer b) {
                return map.get(a)-map.get(b);//用小顶堆存前k多的
            }
        });

        for(Integer key : map.keySet()) {
            if(pq.size() < k){//堆的大小小于k，直接加入堆a
                pq.offer(key);
            }else {//需要判断当前的频率和堆顶元素的哪个大
                if(map.get(pq.peek()) < map.get(key)){//堆顶小，要入堆
                    pq.poll();
                    pq.offer(key);
                }
            }
        }
        int[] ans = new int[pq.size()];
        for(int i=0;i<ans.length;i++) {
            ans[i] = pq.poll();
        }
        return ans;
    }
}
//idea is straightforwd： use hashmap to store the key the its vaulue(frequence)
// sort the value
// when the size of PQ < k, add the key into PQ
// when the size of PQ > k, compare the top one in the PQ, poll(the smallest one fisrt came out) , then offer
// int []
```
时间复杂度：O(nlogk)，n 表示数组的长度。首先，遍历一遍数组统计元素的频率，这一系列操作的时间复杂度是O(n)；接着，遍历用于存储元素频率的 map，如果元素的频率大于最小堆中顶部的元素，则将顶部的元素删除并将该元素加入堆中，这里维护堆的数目是 kk，所以这一系列操作的时间复杂度是O(nlogk) 的；因此，总的时间复杂度是 O(nlog⁡k)。
空间复杂度：O(n)，最坏情况下（每个元素都不同），map 需要存储 n 个键值对，优先队列需要存储 k 个元素，因此，空间复杂度是 O(n)。


