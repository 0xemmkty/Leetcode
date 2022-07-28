268. Missing Number

>Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

## Solution 1 (异或)
时间复杂度：O(n)
空间复杂度：O(1)
>**找缺失数**、**找出现一次数**都是异或的经典应用。
我们可以先求得 [1, n] 的异或和 ans，然后用 ans 对各个 nums[i] 进行异或。
这样最终得到的异或和表达式中，只有缺失元素出现次数为 1 次，其余元素均出现两次**（x⊕x=0）**，即最终答案 ans 为缺失元素

>**异或**，英文为**exclusive OR**，缩写成**xor**异或（xor）

//异或两次能消掉（变为0），最后剩的就是只出现一次的值

``` java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i <= n; i++) ans ^= i;
        for (int i : nums) ans ^= i;
        return ans;
    }
}
```
## Solution 2 (作差)
>利用 nums 的数值范围为 [1,n]，我们可以先计算出 [1,n] 的总和 sum（利用等差数列求和公式），再计算 nums 的总和 cur，两者之间的差值即是 nums 中缺失的数字。
>**时间复杂度**：O(n)
**空间复杂度**：O(1)

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int cur = 0, sum = n * (n + 1) / 2;
        for (int i : nums) cur += i;
        return sum - cur;
    }
}
```




 ## Solution 3 (数组哈希)

 >利用 numsnums 的数值范围为 [0,n][0,n]，且只有一个值缺失，我们可以直接开一个大小为 n + 1n+1 的数组充当哈希表，进行计数，没被统计到的数值即是答案。

 >**时间复杂度**：O(n)
**空间复杂度**：O(n)
```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        boolean[] hash = new boolean[n + 1];
        for (int i = 0; i < n; i++) hash[nums[i]] = true;
        for (int i = 0; i < n; i++) {
            if (!hash[i]) return i;
        }
        return n;
    }
}
```

## Solution 4 (原地哈希)
事实上，我们可以将nums 本身作为哈希表进行使用，将 nums[i] 放到其应该出现的位置（下标）nums[i] 上（ nums[i] <n），然后对 nums 进行检查，找到满足 nums[i] !=i 的位置即是答案，如果不存在 inums[i] !=i 的位置，则 n为答案。


```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] != i && nums[i] < n) swap(nums, nums[i], i--);
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] != i) return i;
        }
        return n;
    }
    void swap(int[] nums, int i, int j) {
        int c = nums[i];
        nums[i] = nums[j];
        nums[j] = c;
    }
}
```
>**时间复杂度**：每个元素会被一次性移动到对应位置，因此每个元素的访问次数为常数次。复杂度为 O(n)
**空间复杂度**：O(1)




## Solution 5 (系统排序)
一个简单的做法是直接对nums 进行排序，找到符合 nums[i] !=i 的位置即是答案，如果不存在 nums[i] !=i 的位置，则 n 为答案。


>**时间复杂度**：假定 Arrays.sort 使用的是双轴快排实现。复杂度为O(nlogn)
**空间复杂度**：O(logn)

 ```java
 class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        for (int i = 0; i < n; i++) {
            if (nums[i] != i) return i;
        }
        return n;
    }
}

 ```