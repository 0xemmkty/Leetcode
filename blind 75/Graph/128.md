# 128. Longest Consecutive Sequence

Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

## Solution 1 （哈希表）
>解题思路1：哈希集合
我们需要优化代码。优化的点主要有两个：
（1）判断num+1，num+2，num+3...是否在数组中。上面的代码是用直接遍历的方式去查找的，时间复杂度为O(n)O(n)，我们可以改为哈希表查找，时间复杂度为O(1)O(1)。
（2）遍历数组中每个元素num。逐一遍历每个元素会产生很多冗余工作，实际上我们无需一次针对每个元素num去判断num+1，num+2，num+3...是否在数组中。如果num-1已经在数组中的话，那么num-1肯定会进行相应的+1遍历，然后遍历到num，而且从num-1开始的+1遍历必定比从num开始的+1遍历得到的序列长度更长。因此，我们便可将在一个连续序列中的元素进行删减，让其只在最小的元素才开始+1遍历。比如，现有元素[1,2,4,3,5]，当2,3,4,5发现均有比自己小1的元素存在，那么它们就不会开始+1遍历，而1是连续序列中最小的元素，没有比自己小1的元素存在，所以会开始+1遍历。通过上述方式便可将时间复杂度优化至O(n)。

>注意：上述代码虽然有两层循环for+while，但是由于if (!set.contains(cur - 1))判断的存在，每个元素只会被遍历一次，因此时间复杂度也为O(n)。



```java
class Solution {
    public int longestConsecutive(int[] nums) {
        // 建立一个存储所有数的哈希表，同时起到去重功能
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }

        int ans = 0;
        // 遍历去重后的所有数字
        for (int num : set) {
            int cur = num;
            // 只有当num-1不存在时，才开始向后遍历num+1，num+2，num+3......
            if (!set.contains(cur - 1)) {
                while (set.contains(cur + 1)) {
                    cur++;
                }
            }
            // [num, cur]之间是连续的，数字有cur - num + 1个
            ans = Math.max(ans, cur - num + 1);
        }
        return ans;
    }
}


```

## Solution 2 (哈希表记录右边界)
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        // key表示num，value表示num最远到达的连续右边界
        Map<Integer, Integer> map = new HashMap<>();
        // 初始化每个num的右边界为自己
        for (int num : nums) {
            map.put(num, num);
        }

        int ans = 0;
        for (int num : nums) {
            if (!map.containsKey(num - 1)) {
                int right = map.get(num);
                // 遍历得到最远的右边界
                while (map.containsKey(right + 1)) {
                    right = map.get(right + 1);
                }
                // 更新右边界
                map.put(num, right);
                // 更新答案
                ans = Math.max(ans, right - num + 1);
            }
            
        }
        return ans;
    }
}
```

## Solution 3(java 内置排序)
```java
class Solution {
    public int longestConsecutive(int[] nums) {  
        if (nums.length == 1) return 1;
        Arrays.sort(nums);
        int max = 0,n = 1;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] + 1 == nums[i + 1]) {
                n += 1;
                max = Math.max(max, n);
            } else if(nums[i] == nums[i + 1]) {
                max = Math.max(max, n);
            }else {
                n = 1;
            }
        }
        return max;
    }
}


```

## Soluion 4 （哈希表 动态规划）
>这是一种非常巧妙的做法，与思路2相同的一点是也利用了Map减小遍历次数。但很重要的一点不同是其value表示的是num所在的连续区间长度。举个例子，当Map的key为5，value为3时，这就表明当前有一个包含5且长度为3的连续区间，当然有多种可能，可以是[3,5],[4,6],[5,7]。

具体做法是：

遍历nums数组中的所有数字num
当num是第一次出现时：
（1）分别获取到左相邻数字num-1的连续区间长度left和右相邻数字num+1的连续区间长度right；
（2）计算得到当前的区间长度为curLen=left+right+1curLen=left+right+1；
（3）更新最长区间长度ans以及左右边界的区间长度。


```java
class Solution {
    public int longestConsecutive(int[] nums) {
        // key表示num，value表示num所在连续区间的长度
        Map<Integer, Integer> map = new HashMap<>();
        int ans = 0;
        for (int num : nums) {
            // 当map中不包含num，也就是num第一次出现
            if (!map.containsKey(num)) {
                // left为num-1所在连续区间的长度，更进一步理解为：左连续区间的长度
                int left = map.getOrDefault(num - 1, 0);
                // right为num+1所在连续区间的长度，更进一步理解为：右连续区间的长度
                int right = map.getOrDefault(num + 1, 0);
                // 当前连续区间的总长度
                int curLen = left + right + 1;
                ans = Math.max(ans, curLen);
                // 将num加入map中，表示已经遍历过该值。其对应的value可以为任意值。
                map.put(num, -1);
                // 更新当前连续区间左边界和右边界对应的区间长度
                map.put(num - left, curLen);
                map.put(num + right, curLen);
            }
        }
        return ans;
    }
}


```

## Solution 5(并查集)
>时间和空间都不太行，但可以开阔思路
>并查集的思路实际上与思路2有点像，也是来记录右边界的，所有在一个连续区间内的元素都会在一个连通分量中，且这些元素的根结点都为最远的右边界元素。
具体思路是：
遍历所有元素num，如果num+1存在，将num加入到num+1所在的连通分量中；
重新遍历一遍所有元素num，通过find函数找到num所在分量的根结点，也就是最远右边界，从而求得连续区间的长度


```java
class UnionFind {
    // 记录每个节点的父节点
    private Map<Integer, Integer> parent;

    public UnionFind(int[] nums) {
        parent = new HashMap<>();
        // 初始化父节点为自身
        for (int num : nums) {
            parent.put(num, num);
        }
    }

    // 寻找x的父节点，实际上也就是x的最远连续右边界，这点类似于方法2
    public Integer find(int x) {
        // nums不包含x
        if (!parent.containsKey(x)) {
            return null;
        }
        // 遍历找到x的父节点
        while (x != parent.get(x)) {
            // 进行路径压缩，不写下面这行也可以，但是时间会慢些
            parent.put(x, parent.get(parent.get(x)));
            x = parent.get(x);
        }
        return x;
    }

    // 合并两个连通分量，在本题中只用来将num并入到num+1的连续区间中
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            return;
        }
        parent.put(rootX, rootY);
    }
}

class Solution {
    public int longestConsecutive(int[] nums) {
        UnionFind uf = new UnionFind(nums);
        int ans = 0;
        
        for (int num : nums) {
            // 当num+1存在，将num合并到num+1所在集合中
            if (uf.find(num + 1) != null) {
                uf.union(num, num + 1);
            }
        }

        for (int num : nums) {
            // 找到num的最远连续右边界
            int right = uf.find(num);
            ans = Math.max(ans, right - num + 1);
        }
        return ans;
    }
}

```

