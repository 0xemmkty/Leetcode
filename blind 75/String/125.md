# 125. Valid Palindrome

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

## Solution 1(double pointer)
```java
class Solution {
     public static boolean isPalindrome(String s) {
        StringBuffer sb = new StringBuffer();
        for (int i = 0;i < s.length();i++) {
            char temp = s.charAt(i);
            if (temp >= '0' && temp <= '9') {
                sb.append(temp);
            }else if (temp >= 'a' && temp <= 'z') {
                sb.append(temp);
            }else if (temp >= 'A' && temp <= 'Z') {
                sb.append((char)(temp+32));
            }
        }
        int i = 0, j = sb.length()-1;
        while (i <= j) {
            if (sb.charAt(i) == sb.charAt(j)) {
                i++;
                j--;
            }else {
                return false;
            }
        }

        return true;
}
}
//注意数据的转换，大写转换成小写
// (char)(temp+32)
// 双指针部分简单
```