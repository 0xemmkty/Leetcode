# 1899. Merge Triplets to Form Target Triplet

>A triplet is an array of three integers. You are given a 2D integer array triplets, where triplets[i] = [ai, bi, ci] describes the ith triplet. You are also given an integer array target = [x, y, z] that describes the triplet you want to obtain.

>To obtain target, you may apply the following operation on triplets any number of times (possibly zero):

>Choose two indices (0-indexed) i and j (i != j) and update triplets[j] to become [max(ai, aj), max(bi, bj), max(ci, cj)].
For example, if triplets[i] = [2, 5, 3] and triplets[j] = [1, 7, 5], triplets[j] will be updated to [max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5].
Return true if it is possible to obtain the target triplet [x, y, z] as an element of triplets, or false otherwise.

## Soution 1
```java
class Solution {
    public boolean mergeTriplets(int[][] triplets, int[] target) {
        int a = 0, b = 0, c = 0;

        for (int[] t : triplets) {
            if (t[0] <= target[0] && t[1] <= target[1] && t[2] <= target[2]) {
                a = Math.max(a, t[0]);
                b = Math.max(b, t[1]);
                c = Math.max(c, t[2]);
                if (a == target[0] && b == target[1] && c == target[2]) {
                    return true;
                }
            }
        }
        return false;
    }
}


```
时间复杂度：O(n)，其中 n 是数组 triples 的长度。

空间复杂度：O(1)。
