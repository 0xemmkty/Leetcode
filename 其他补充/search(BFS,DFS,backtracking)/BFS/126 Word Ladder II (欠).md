# 126. Word Ladder II (欠)

A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

Every adjacent pair of words differs by a single letter.
Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
sk == endWord
Given two words, beginWord and endWord, and a dictionary wordList, return all the shortest transformation sequences from beginWord to endWord, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words [beginWord, s1, s2, ..., sk].

 
```
Example 1:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
Explanation: There are 2 shortest transformation sequences:
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"
```

## SOlution 1(BFS)
超时
```
BFS
思路说明：在到达最短路径所在的层时，记录并输出所有符合条件的路径。

在单词接龙的基础上，需要将找到的最短路径存储下来；
之前的队列只用来存储每层的元素，那么现在就得存储每层添加元素之后的结果："ab","if",{"cd","af","ib","if"}；
（1）第一层：{"ab"}
（2）第二层：{"ab","af"}、{"ab","ib"}
（3）第三层：{"ab","af","if"}、{"ab","ib","if"}
如果该层添加的某一个单词符合目标单词，则该路径为最短路径，该层为最短路径所在的层，但此时不能直接返回结果，必须将该层遍历完，将该层所有符合的结果都添加进结果集；
每层添加单词的时候，不能直接添加到总的已访问单词集合中，需要每层有一个单独的该层访问的单词集，该层结束之后，再会合到总的已访问单词集合中，原因就是因为3.
```




## Solution 2(双向BFS+DFS)
超时
```java
双向BFS + DFS
思路说明：利用双向BFS构建出每个单词可达的下层单词，之后根据该关系利用DFS构建路径，输出符合条件的路径即可。

单词关系：始终记录该单词的可达的所有下层关系："ab","if",{"cd","af","ib","if"}
（1）"ab" -> {"af", "ib"}
（2）"af" -> {"if"}
（3）"ib" -> {"if"}
探索顺序：
（1）从上到下：只需在关系map中，添加可达的单词即可
（2）从下到上：当前遍历的单词，与该单词相差一个字符的所有单词，因为是从下到上，所以当前遍历的单词是得到的相差一个字符的单词的可达下层单词




