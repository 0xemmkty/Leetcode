# 131. Palindrome Partitioning
(分割回文串)

>Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

>A palindrome string is a string that reads the same backward as forward.

## Solution 1
```java
class Solution {

    //声明一个全局变量ans
    List<List<String>> ans = new ArrayList<>();

    //主方法
    public List<List<String>> partition(String s) {
        backtrack(s.toCharArray(), 0, new ArrayList());
        return ans;
    }

    //回溯方法
    void backtrack(char[] s, int startIndex, List<String> path) {
        //当startIndex越界时返回
        if(startIndex == s.length) {
            //将当前path添加到ans中，注意：需要新造一个ArrayList
            ans.add(new ArrayList(path));
            return;
        }
        //从startIndex开始枚举
        for(int i = startIndex; i < s.length; i++) {
            if(check(s, startIndex, i)) {
                //从startIndex到i如果是回文串，则将此串加入path，并向下递归
                path.add(new String(s, startIndex, i - startIndex + 1));
                backtrack(s, i + 1, path);
                //递归完毕后，记得将此串从path中删除，这是回溯算法的关键步骤
                path.remove(path.size() - 1);
            }
        }
    }

    //辅助方法，判断从start到end是否回文
    boolean check(char[] s, int start, int end) {
        while(start <= end) {
            if(s[start++] != s[end--]) {
                return false;
            }
        }
        return true;
    }
}

```


 