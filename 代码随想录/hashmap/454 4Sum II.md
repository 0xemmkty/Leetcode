# 454. 4Sum II

>Given four integer arrays nums1, nums2, nums3, and nums4 all of length n, return the number of tuples (i, j, k, l) such that:

>0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

```
Example 1:

Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
Output: 2
Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

## Solution 1 (hash)
解题思路
思路：

一采用分为两组，HashMap 存一组，另一组和 HashMap 进行比对。
这样的话情况就可以分为三种：
HashMap 存一个数组，如 A。然后计算三个数组之和，如 BCD。时间复杂度为：O(n)+O(n^3)，得到 O(n^3).
HashMap 存三个数组之和，如 ABC。然后计算一个数组，如 D。时间复杂度为：O(n^3)+O(n)，得到 O(n^3).
HashMap存两个数组之和，如AB。然后计算两个数组之和，如 CD。时间复杂度为：O(n^2)+O(n^2)，得到 O(n^2).
根据第二点我们可以得出要存两个数组算两个数组。
我们以存 AB 两数组之和为例。首先求出 A 和 B 任意两数之和 sumAB，以 sumAB 为 key，sumAB 出现的次数为 value，存入 hashmap 中。
然后计算 C 和 D 中任意两数之和的相反数 sumCD，在 hashmap 中查找是否存在 key 为 sumCD。
算法时间复杂度为 O(n2)。

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer, Integer> map = new HashMap<>();
        //Map<Integer, Integer> map = new HashMap<>();
        int res = 0;
        for(int i = 0;i<A.length;i++){
            for(int j= 0;j<B.length;j++){
                int sumAB = A[i]+B[j];
                if(map.containsKey(sumAB)) map.put(sumAB,map.get(sumAB)+1);
                else map.put(sumAB,1);
            }
        }

        for(int i = 0;i<C.length;i++){
            for(int j = 0;j<D.length;j++){
                int sumCD = -(C[i]+D[j]);
                if(map.containsKey(sumCD)) res += map.get(sumCD);
            }
        }
        return res;
    }
}

```

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        int temp;
        int res = 0;
        //统计两个数组中的元素之和，同时统计出现的次数，放入map
        for (int i : nums1) {
            for (int j : nums2) {
                temp = i + j;
                if (map.containsKey(temp)) {
                    map.put(temp, map.get(temp) + 1);
                } else {
                    map.put(temp, 1);
                }
            }
        }
        //统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                temp = i + j;
                if (map.containsKey(0 - temp)) {
                    res += map.get(0 - temp);
                }
            }
        }
        return res;
    }
}
```
时间复杂度：O(n^2)。我们使用了两次二重循环，时间复杂度均为 O(n^2)。在循环中对哈希映射进行的修改以及查询操作的期望时间复杂度均为 O(1)O(1)，因此总时间复杂度为 O(n^2)。

空间复杂度：O(n^2)，即为哈希映射需要使用的空间。在最坏的情况下，A[i]+B[j]A[i]+B[j] 的值均不相同，因此值的个数为 n^2，也就需要 O(n^2)的空间。



