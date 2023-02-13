# 72. Edit Distance

>Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.
You have the following three operations permitted on a word:

>Insert a character
Delete a character
Replace a character
 
```
Example 1:

Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```
## Solution 1
![avatar](../pic/11.png)
非常漂亮的题解：
```
讲一下我自己对状态转移方程的理解,麻烦大家看看我说得对不对有没有道理:
(一)、当word1[i]==word2[j]时,由于遍历到了i和j,说明word1的0~i-1和word2的0~j-1的匹配结果已经生成,
由于当前两个字符相同,因此无需做任何操作,dp[i][j]=dp[i-1][j-1]
(二)、当word1[i]!=word2[j]时,可以进行的操作有3个:
      ① 替换操作:可能word1的0~i-1位置与word2的0~j-1位置的字符都相同,
           只是当前位置的字符不匹配,进行替换操作后两者变得相同,
           所以此时dp[i][j]=dp[i-1][j-1]+1(这个加1代表执行替换操作)
      ②删除操作:若此时word1的0~i-1位置与word2的0~j位置已经匹配了,
         此时多出了word1的i位置字符,应把它删除掉,才能使此时word1的0~i(这个i是执行了删除操作后新的i)
         和word2的0~j位置匹配,因此此时dp[i][j]=dp[i-1][j]+1(这个加1代表执行删除操作)
      ③插入操作:若此时word1的0~i位置只是和word2的0~j-1位置匹配,
          此时只需要在原来的i位置后面插入一个和word2的j位置相同的字符使得
          此时的word1的0~i(这个i是执行了插入操作后新的i)和word2的0~j匹配得上,
          所以此时dp[i][j]=dp[i][j-1]+1(这个加1代表执行插入操作)
      ④由于题目所要求的是要最少的操作数:所以当word1[i] != word2[j] 时,
          需要在这三个操作中选取一个最小的值赋格当前的dp[i][j]
(三)总结:状态方程为:
if(word1[i] == word2[j]):
      dp[i][j] = dp[i-1][j-1]
else:
       min(dp[i-1][j-1],dp[i-1][j],dp[i][j-1])+1


PS:大佬的代码中word1.charAt(i-1)==word2.charAt(j-1)的原因是:
     初始化DP Table时dp[i][0]和dp[0][j]已经填写完成,所以接下来填表需要从1开始,
     但是字符的比较需要从0开始,因此才这样子写
```
### 自底向上
java:
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int n1 = word1.length();
        int n2 = word2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];
        // 第一行
        for (int j = 1; j <= n2; j++) dp[0][j] = dp[0][j - 1] + 1;
        // 第一列
        for (int i = 1; i <= n1; i++) dp[i][0] = dp[i - 1][0] + 1;

        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = Math.min(Math.min(dp[i - 1][j - 1], dp[i][j - 1]), dp[i - 1][j]) + 1;
            }
        }
        return dp[n1][n2];  
    }
}

```
python:
```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        n1 = len(word1)
        n2 = len(word2)
        dp = [[0] * (n2 + 1) for _ in range(n1 + 1)]
        # 第一行
        for j in range(1, n2 + 1):
            dp[0][j] = dp[0][j-1] + 1
        # 第一列
        for i in range(1, n1 + 1):
            dp[i][0] = dp[i-1][0] + 1
        for i in range(1, n1 + 1):
            for j in range(1, n2 + 1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1] ) + 1
        #print(dp)      
        return dp[-1][-1]
```

### 自顶向下
```python
import functools
class Solution:
    @functools.lru_cache(None)
    def minDistance(self, word1: str, word2: str) -> int:
        if not word1 or not word2:
            return len(word1) + len(word2)
        if word1[0] == word2[0]:
            return self.minDistance(word1[1:], word2[1:])
        else:
            inserted = 1 + self.minDistance(word1, word2[1:])
            deleted = 1 + self.minDistance(word1[1:], word2)
            replace = 1 + self.minDistance(word1[1:], word2[1:])
            return min(inserted, deleted, replace)
```
```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        import functools
        @functools.lru_cache(None)
        def helper(i, j):
            if i == len(word1) or j == len(word2):
                return len(word1) - i + len(word2) - j
            if word1[i] == word2[j]:
                return helper(i + 1, j + 1)
            else:
                inserted = helper(i, j + 1)
                deleted = helper(i + 1, j)
                replaced = helper(i + 1, j + 1)
                return min(inserted, deleted, replaced) + 1
        return helper(0, 0)

```