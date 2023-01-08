# 349. Intersection of Two Arrays

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

## Solution 1
但是要注意，使用数组来做哈希的题目，是因为题目都限制了数值的大小。

而这道题目没有限制数值的大小，就无法使用数组来做哈希表了。

而且如果哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费。

此时就要使用另一种结构体了，set ，

后记
本题后面 力扣改了 题目描述 和 后台测试数据，增添了 数值范围：

1 <= nums1.length, nums2.length <= 1000
0 <= nums1[i], nums2[i] <= 1000
所以就可以 使用数组来做哈希表了， 因为数组都是 1000以内的。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) {
            return new int[0];
        }
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resSet = new HashSet<>();
        //遍历数组1
        for (int i : nums1) {
            set1.add(i);
        }
        //遍历数组2的过程中判断哈希表中是否存在该元素
        for (int i : nums2) {
            if (set1.contains(i)) {
                resSet.add(i);
            }
        }
        //将结果几何转为数组
        return resSet.stream().mapToInt(x -> x).toArray();
    }
}

```

 