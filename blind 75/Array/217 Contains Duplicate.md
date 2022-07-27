#217. Contains Duplicate


Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

##方法一 排序
23ns
- 数组排序后，判断相邻元素是否相等。
时间复杂度 : O(nlogn)。即排序的时间复杂度。扫描的时间复杂度 O(n) 可忽略。

* 空间复杂度 : O(1)。 没有用到额外空间。如果深究 Arrays.sort(nums) 使用了栈空间，那就是O(logn)。



    class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
    }
    //Arrays.sort(nums);
    //++i is faster than i++


##方法二 使用 set
19ms
遍历数组，数字放到 set 中。如果数字已经存在于 set 中，直接返回 true。如果成功遍历完数组，则表示没有重复元素，返回 false。

    class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num: nums) {
            if (set.contains(num)) {
                return true;
            }
            set.add(num);
        }
        return false;
    }
}



 