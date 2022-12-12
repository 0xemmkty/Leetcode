## 567. Permutation in String (待)

>Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.

>In other words, return true if one of s1's permutations is the substring of s2.

## Solution 1 (sliding window)
题目没有昨天的题那么费脑子。滑动窗口，双指针 left、right交替向右滑动即可。

客观上来说，最终的答案如果是true，则s2中必然存在某个位置 i 使得以 i 开始的子串s2[i ... i+len1)是s1的排列。

依次尝试固定以s2中的每一个位置 l 作为左端点开始的len1长度的子串s2[l ... l+len1)是否是s1的排列，最终必然不会错过所有的可能性。

如果固定 l 做左端点，尝试失败，继续尝试 l+1 位置，【左右边界都不需要回退】，这是滑动窗口的前提。

通过一个记账本 charCount 做为【总账表】维护s1的词频表；
滑动窗口内每一个右边界字符进入窗口后，【还账】：charCount[str2[r] - 'a']--
如果某个字符多还了（变成负值），即尝试失败，开始尝试下一个左端点（l++）;
左边界字符出窗口后，表示【重新赊账】：charCount[str2[l] - 'a']++
最终如果欠账还足了（窗口长度达到len1），则尝试成功，直接返回true。
时间：O(N)，Java，击败：100%
```java
public static boolean checkInclusion(String s1, String s2) {
        char[] str1 = s1.toCharArray();
        char[] str2 = s2.toCharArray();
        int len1 = s1.length();
        int len2 = s2.length();
        int[] charCount = new int[26]; // 【总欠账表】：s1的词频表
        for (char c : str1) { // 统计s1的词频
            charCount[c - 'a']++;
        }
        int l = 0, r = 0; // 滑动窗口左右边界
        // 依次尝试固定以s2中的每一个位置l作为左端点开始的len1长度的子串s2[l ... l+len1)是否是s1的排列
        while (l <= len2 - len1) { // 固定左端点只需要尝试到len2-len1即可
            // 右边界s2[r]字符进入窗口【还账】
            while (r < l + len1 && charCount[str2[r] - 'a'] >= 1) {
                charCount[str2[r] - 'a']--; // 【"还账"】
                r++;
            }
            if (r == l + len1) return true;
            // 左边界s2[l]字符出窗口【赊账】，l++，开始尝试固定下一个位置做左端点
            charCount[str2[l] - 'a']++; // 重新【"赊账"】
            l++;
        }
        return false; // 所有的左端点均尝试还账失败，不可能再有答案了
    }
```
```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        char[] s11=s1.toCharArray();
        char[] s22=s2.toCharArray();
        int n1=s1.length();
        int n2=s2.length();
        if(n2<n1)
        return false;
        int[] counter1=new int[26];
        int[] counter2=new int[26];
        // create the window
        int left=0;
        int right=left+n1-1;
        for(int i=0;i<n1;i++)
        {
            counter1[s11[i]-'a']++;
        }
        for(int i=0;i<n1-1;i++)
        {
            counter2[s22[i]-'a']++;
        }        
        while(right<n2)
        {          
            counter2[s22[right]-'a']++;
            if(Arrays.equals(counter1,counter2))
                return true;           
            else
            {       
                counter2[s22[left]-'a']--;   
                if(counter2[s22[left]-'a']<0)
                counter2[s22[left]-'a']=0;

                left++;
                right++;
            }
        }
        return false;
    }
}
```

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
       int[] count = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            count[s1.charAt(i) - 'a']++;
        }
        int left = 0, right = 0, len = s1.length();
        while (right < s2.length()) {
            if (count[s2.charAt(right++) - 'a']-- >= 1) {
                len--;
            }
            if (len == 0) return true;
            if (right - left == s1.length() && count[s2.charAt(left++) - 'a']++ >= 0) {
                len++;
            }
        }
        return false;
    }

}
```

76,567,438三道题可以用一套模板解决！！！——出自labuladong，东哥yyds！
```java
//框架
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> window = new HashMap<>();
    for (char c : t.toCharArray()) 
        need.put(c,need.getOrDefault(c,0)+1);
	int left = 0, right = 0;
	int valid = 0; 
	while (right < s.size()) {
    	// c 是将移入窗口的字符
   	 	char c = s.charAt(right);
    	// 右移窗口
    	right++;
    	// 进行窗口内数据的一系列更新
    	...

    	/*** debug 输出的位置 ***/
    	System.out.println("window: ["+left+","+ right+")");
    	/********************/
    
    	// 判断左侧窗口是否要收缩
    	while (window needs shrink) {
        	// d 是将移出窗口的字符
        	char d = s[left];
        	// 左移窗口
        	left++;
        	// 进行窗口内数据的一系列更新
        	...
    	}
	}
}

//题解
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        Map<Character,Integer> need = new HashMap<>();
        Map<Character,Integer> window = new HashMap<>();    
        for(char c : s1.toCharArray()){
            need.put(c,need.getOrDefault(c,0)+1);
        }    
        int left = 0,right = 0,count = 0;
        while(right < s2.length()){
            char c = s2.charAt(right);
            right++;
            if(need.containsKey(c)){
                window.put(c,window.getOrDefault(c,0)+1);
                if(need.get(c).equals(window.get(c))){
                    count++;
                }
            }
            //只有在right - left == need.size()的情况下才有可能有符合条件的解
            while(right - left == s1.length()){
                if(count == need.size())
                    return true;
                char d = s2.charAt(left);
                left++;
                if(need.containsKey(d)){
                    if(need.get(d).equals(window.get(d))){
                        count--;
                    }
                    window.put(d,window.getOrDefault(d,0)-1);
                }
            }
        }
        return false;
    }
}
```
