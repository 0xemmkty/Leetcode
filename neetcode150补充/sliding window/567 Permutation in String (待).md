## 567. Permutation in String

>Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.

>In other words, return true if one of s1's permutations is the substring of s2.

## Solution 1 (sliding window)
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

