# 217. Contains Duplicate

>Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

## Solution 1
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        // Create a hashset...
        HashSet<Integer> hset = new HashSet<Integer>();
        // Traverse all the elements through the loop...
        for (int idx = 0; idx < nums.length; idx ++){
            // Searches hashset. if present, it contains duplicate...
            if (hset.contains(nums[idx])){
                return true;
            }
            // if not present it will update hashset...
            hset.add(nums[idx]);
        }
        // Otherwise return false...
        return false;
    }
}
```
O(n)

## Solution 2
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        // Base case...
        if(nums==null || nums.length==0)
            return false;
        // Create a hashset...
        HashSet<Integer> hset = new HashSet<Integer>();
        // Traverse all the elements through the loop...
        for(int idx: nums){
            // If it contains duplicate...
            if(!hset.add(idx)){
                return true;
            }
        }
        // Otherwise return false...
        return false;
    }
}
```
o(n)