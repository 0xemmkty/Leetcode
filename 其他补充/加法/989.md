# 989. Add to Array-Form of Integer 数组形式的整数加法
(array add)

题目地址：https://leetcode.com/problems/add-to-array-form-of-integer/

# 题目描述
For a non-negative integer X, the array-form of X is an array of its digits in left to right order. For example, if X = 1231, then the array form is [1,2,3,1].

Given the array-form A of a non-negative integer X, return the array-form of the integer X+K.

Example 1:
Input: A = [1,2,0,0], K = 34
Output: [1,2,3,4]
Explanation: 1200 + 34 = 1234

## Solution 1
```java
class Solution {
    public List<Integer> addToArrayForm(int[] num, int k) {
        List<Integer> res = new ArrayList<>(); // 返回结果
        // one of the differenies between array and linklist is
        // how to find the index
        // for linklist, just node.next for moving
        // for array, we need use length-1 to find certain num position
        // and use - to reach what we want
        // also k is a whole num 
        int p1 = num.length - 1; // 标记遍历到 num 的位置
        int carry = 0; // 进位
        while (p1 >= 0 || k != 0 || carry != 0) { // num 没遍历完，或 k 没遍历完，或进位不为 0
            int adder1 = p1 >= 0 ? num[p1] : 0; // 当前 num 的取值
            int adder2 = k % 10; // 当前 k 的位置，如果 k 已经是 0 那么 % 10 以后仍然是 0
            int sum = adder1 + adder2 + carry; // 当前位置相加的结果
            carry = sum >= 10 ? 1 : 0; // 是否有进位
            sum = sum >= 10 ? sum - 10 : sum; // 去除进位后留下的数字
            res.add(sum); // 把去除进位后留下的数字拼接到结果中
            p1 --; // 遍历到 num 的位置向左移动
            k /= 10; // 取 k 的下一个位置的数字
        }
        Collections.reverse(res); // 把结果反转
        return res; 
    }
}
```