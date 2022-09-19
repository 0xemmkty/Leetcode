# 3. Longest Substring Without Repeating Characters

Given a string s, find the **length** of the **longest substring without repeating characters**.

## Solution 1 (滑动窗口)
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max = 0;//最长子串长度
        int left = 0;//滑动窗口左下标，i相当于滑动窗口右下标
        for(int i = 0; i < s.length(); i ++){
            if(map.containsKey(s.charAt(i))){//charAt() 方法用于返回指定索引处的字符。索引范围为从 0 到 length() - 1。
                left = Math.max(left,map.get(s.charAt(i)) + 1);       //map.get():返回字符所对应的索引，当发现重复元素时，窗口左指针右移
            }        //map.get('a')=0,因为map中只有第一个a的下标，然后更新left指针到原来left的的下一位
            map.put(s.charAt(i),i);      //再更新map中a映射的下标
            max = Math.max(max,i-left+1);     //比较两个参数的大小
        }
        return max;
        
    }
}
```
```
时间复杂度：O(N)，其中 N 是字符串的长度。左指针和右指针分别会遍历整个字符串一次。

空间复杂度：O(∣Σ∣)，其中 Σ 表示字符集（即字符串中可以出现的字符），∣Σ∣ 表示字符集的大小。在本题中没有明确说明字符集，因此可以默认为所有 ASCII 码在 [0, 128)[0,128) 内的字符，即 ∣Σ∣=128。我们需要用到哈希集合来存储出现过的字符，而字符最多有 ∣Σ∣ 个，因此空间复杂度为 O(∣Σ∣)。


```

## Solution 2 (双指针+哈希表) 
不推荐 仅作用于拓展思路
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length() == 0)return 0;
        int ans = 1;
        int[]hash = new int[200];
        hash[s.charAt(0)] = 1;
        for(int i = 0,j = 0;i <= j && j < s.length();)
        {
            boolean flag = false;
            for(int k = 0 ;k < hash.length ;k++)
                if(hash[k] > 1){flag = true;break;}
            if(!flag)
            {
                ans = Math.max(ans,j-i+1);
                j++;
                if(j < s.length())
                hash[s.charAt(j)]++;
                else break;
            }
            else
            {
                hash[s.charAt(i)]--;
                i++;
            }
            
        }
        return ans;
        
    }
}
```