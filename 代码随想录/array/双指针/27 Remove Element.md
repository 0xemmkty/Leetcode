## 27. Remove Element

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The relative order of the elements may be changed.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

一个需要知道的重点：
数组的元素在内存地址中是连续的，不能单独删除数组中的某个元素，只能覆盖。
因此，所谓的删除并不是真的指从数组中抹去，而是覆盖。即下一位覆盖前一位。

题目的一个前提：
说明:

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
 

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/remove-element
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
## Solution 1 （暴力破解）
```c
// 时间复杂度：O(n^2)
// 空间复杂度：O(1)
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (nums[i] == val) { // 发现需要移除的元素，就将数组集体向前移动一位
                for (int j = i + 1; j < size; j++) {
                    nums[j - 1] = nums[j];
                }
                i--; // 因为下标i以后的数值都向前移动了一位，所以i也向前移动一位
                size--; // 此时数组的大小-1
            }
        }
        return size;

    }
};
```

## Solution 2 （双指针）
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int n = nums.length;
        int left = 0;
        for (int right = 0; right < n; right++) {
            // 这里 if，如果nums[right] = val， left就原地不动，
            // 但是right是一只加的， 就会出现“错位”和“覆盖”
            if (nums[right] != val) {
                nums[left] = nums[right];
                left++;
            }
        }
        // 以及，注意为什么这里明明返回的时一个值实际上返回的确实数组呢
        return left;
    }
}

```
时间复杂度：O(n)，其中 n 为序列的长度。我们只需要遍历该序列至多两次。

空间复杂度：O(1)。我们只需要常数的空间保存若干变量。

## Solution 3 （双指针 优化）
```
方法二：双指针优化
思路

如果要移除的元素恰好在数组的开头，例如序列 [1,2,3,4,5][1,2,3,4,5]，当 \textit{val}val 为 11 时，我们需要把每一个元素都左移一位。注意到题目中说：「元素的顺序可以改变」。实际上我们可以直接将最后一个元素 55 移动到序列开头，取代元素 11，得到序列 [5,2,3,4][5,2,3,4]，同样满足题目要求。这个优化在序列中 \textit{val}val 元素的数量较少时非常有效。

实现方面，我们依然使用双指针，两个指针初始时分别位于数组的首尾，向中间移动遍历该序列。

算法

如果左指针 \textit{left}left 指向的元素等于 \textit{val}val，此时将右指针 \textit{right}right 指向的元素复制到左指针 \textit{left}left 的位置，然后右指针 \textit{right}right 左移一位。如果赋值过来的元素恰好也等于 \textit{val}val，可以继续把右指针 \textit{right}right 指向的元素的值赋值过来（左指针 \textit{left}left 指向的等于 \textit{val}val 的元素的位置继续被覆盖），直到左指针指向的元素的值不等于 \textit{val}val 为止。

当左指针 \textit{left}left 和右指针 \textit{right}right 重合的时候，左右指针遍历完数组中所有的元素。

这样的方法两个指针在最坏的情况下合起来只遍历了数组一次。与方法一不同的是，方法二避免了需要保留的元素的重复赋值操作

作者：LeetCode-Solution
链接：https://leetcode.cn/problems/remove-element/solution/yi-chu-yuan-su-by-leetcode-solution-svxi/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```