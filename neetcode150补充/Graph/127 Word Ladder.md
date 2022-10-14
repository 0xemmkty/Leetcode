# 127. Word Ladder (先欠，双向BFS)

>A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

>**Every adjacent pair of words differs by a single letter**.
Every si for 1 <= i <= k is in wordList. Note that **beginWord does not need to be in wordList**.
sk == endWord
Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the **shortest transformation sequence** from beginWord to endWord, or 0 if no such sequence exists.

## Solution 1 (BFS)
如果一开始就构建图，每一个单词都需要和除它以外的另外的单词进行比较，复杂度是 O(N \rm{wordLen})O(NwordLen)，这里 NN 是单词列表的长度；
为此，我们在遍历一开始，把所有的单词列表放进一个哈希表中，然后在遍历的时候构建图，每一次得到在单词列表里可以转换的单词，复杂度是 O(26 \times \rm{wordLen})O(26×wordLen)，借助哈希表，找到邻居与 NN 无关；
使用 BFS 进行遍历，需要的辅助数据结构是：
队列；
visited 集合。说明：可以直接在 wordSet (由 wordList 放进集合中得到)里做删除。但更好的做法是新开一个哈希表，遍历过的字符串放进哈希表里。这种做法具有普遍意义。绝大多数在线测评系统和应用场景都不会在意空间开销。


```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Set;

public class Solution {

    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // 第 1 步：先将 wordList 放到哈希表里，便于判断某个单词是否在 wordList 里
        Set<String> wordSet = new HashSet<>(wordList);
        if (wordSet.size() == 0 || !wordSet.contains(endWord)) {
            return 0;
        }
        wordSet.remove(beginWord);
        
        // 第 2 步：图的广度优先遍历，必须使用队列和表示是否访问过的 visited 哈希表
        Queue<String> queue = new LinkedList<>();
        queue.offer(beginWord);
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        
        // 第 3 步：开始广度优先遍历，包含起点，因此初始化的时候步数为 1
        int step = 1;
        while (!queue.isEmpty()) {
            int currentSize = queue.size();
            for (int i = 0; i < currentSize; i++) {
                // 依次遍历当前队列中的单词
                String currentWord = queue.poll();
                // 如果 currentWord 能够修改 1 个字符与 endWord 相同，则返回 step + 1
                if (changeWordEveryOneLetter(currentWord, endWord, queue, visited, wordSet)) {
                    return step + 1;
                }
            }
            step++;
        }
        return 0;
    }

    /**
     * 尝试对 currentWord 修改每一个字符，看看是不是能与 endWord 匹配
     *
     * @param currentWord
     * @param endWord
     * @param queue
     * @param visited
     * @param wordSet
     * @return
     */
    private boolean changeWordEveryOneLetter(String currentWord, String endWord,
                                             Queue<String> queue, Set<String> visited, Set<String> wordSet) {
        char[] charArray = currentWord.toCharArray();
        for (int i = 0; i < endWord.length(); i++) {
            // 先保存，然后恢复
            char originChar = charArray[i];
            for (char k = 'a'; k <= 'z'; k++) {
                if (k == originChar) {
                    continue;
                }
                charArray[i] = k;
                String nextWord = String.valueOf(charArray);
                if (wordSet.contains(nextWord)) {
                    if (nextWord.equals(endWord)) {
                        return true;
                    }
                    if (!visited.contains(nextWord)) {
                        queue.add(nextWord);
                        // 注意：添加到队列以后，必须马上标记为已经访问
                        visited.add(nextWord);
                    }
                }
            }
            // 恢复
            charArray[i] = originChar;
        }
        return false;
    }
}
```

## Solution 2 (双向BFS)
已知目标顶点的情况下，可以分别从起点和目标顶点（终点）执行广度优先遍历，直到遍历的部分有交集。这种方式搜索的单词数量会更小一些；
更合理的做法是，每次从单词数量小的集合开始扩散；
这里 beginVisited 和 endVisited 交替使用，等价于单向 BFS 里使用队列，每次扩散都要加到总的 visited 里。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {

    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // 第 1 步：先将 wordList 放到哈希表里，便于判断某个单词是否在 wordList 里
        Set<String> wordSet = new HashSet<>(wordList);
        if (wordSet.size() == 0 || !wordSet.contains(endWord)) {
            return 0;
        }

        // 第 2 步：已经访问过的 word 添加到 visited 哈希表里
        Set<String> visited = new HashSet<>();
        // 分别用左边和右边扩散的哈希表代替单向 BFS 里的队列，它们在双向 BFS 的过程中交替使用
        Set<String> beginVisited = new HashSet<>();
        beginVisited.add(beginWord);
        Set<String> endVisited = new HashSet<>();
        endVisited.add(endWord);

        // 第 3 步：执行双向 BFS，左右交替扩散的步数之和为所求
        int step = 1;
        while (!beginVisited.isEmpty() && !endVisited.isEmpty()) {
            // 优先选择小的哈希表进行扩散，考虑到的情况更少
            if (beginVisited.size() > endVisited.size()) {
                Set<String> temp = beginVisited;
                beginVisited = endVisited;
                endVisited = temp;
            }

            // 逻辑到这里，保证 beginVisited 是相对较小的集合，nextLevelVisited 在扩散完成以后，会成为新的 beginVisited
            Set<String> nextLevelVisited = new HashSet<>();
            for (String word : beginVisited) {
                if (changeWordEveryOneLetter(word, endVisited, visited, wordSet, nextLevelVisited)) {
                    return step + 1;
                }
            }

            // 原来的 beginVisited 废弃，从 nextLevelVisited 开始新的双向 BFS
            beginVisited = nextLevelVisited;
            step++;
        }
        return 0;
    }


    /**
     * 尝试对 word 修改每一个字符，看看是不是能落在 endVisited 中，扩展得到的新的 word 添加到 nextLevelVisited 里
     *
     * @param word
     * @param endVisited
     * @param visited
     * @param wordSet
     * @param nextLevelVisited
     * @return
     */
    private boolean changeWordEveryOneLetter(String word, Set<String> endVisited,
                                             Set<String> visited,
                                             Set<String> wordSet,
                                             Set<String> nextLevelVisited) {
        char[] charArray = word.toCharArray();
        for (int i = 0; i < word.length(); i++) {
            char originChar = charArray[i];
            for (char c = 'a'; c <= 'z'; c++) {
                if (originChar == c) {
                    continue;
                }
                charArray[i] = c;
                String nextWord = String.valueOf(charArray);
                if (wordSet.contains(nextWord)) {
                    if (endVisited.contains(nextWord)) {
                        return true;
                    }
                    if (!visited.contains(nextWord)) {
                        nextLevelVisited.add(nextWord);
                        visited.add(nextWord);
                    }
                }
            }
            // 恢复，下次再用
            charArray[i] = originChar;
        }
        return false;
    }
}

```
另一版本
```java
class Solution {
    /*
    总体思路:双向BFS(参考liweiwei)
    BFS是一圈一圈地进行扫描,因此可以在最先到达终点时计算出扫描的圈数,这个圈数就是最短路径长度
    为了避免重复扫描,还需要一个容器来标记已经搜索过的节点
    双向BFS比单向BFS搜索的节点更少,因为每深一层,节点的数目都会以指数趋势增长
     */
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // 存储字典中的单词+去重(List->Set)
        Set<String> wordSet = new HashSet<>(wordList);
        // 去除开始单词
        wordSet.remove(beginWord);
        // 若wordSet没包含endWord直接到达不了终点返回0
        if (wordSet.size() == 0 || !wordSet.contains(endWord)) {
            return 0;
        }
        // 辅助哈希表1:从begin开始的正向搜索
        // 这里存储的是当前层搜到的节点(注意是单层的)
        Set<String> beginSet = new HashSet<>() {{
            add(beginWord);
        }};
        // 辅助哈希表2:从endWord开始的反向搜索
        Set<String> endSet = new HashSet<>() {{
            add(endWord);
        }};
        // 标记已经搜索过的单词(正+反)
        Set<String> visited = new HashSet<>() {{
            add(beginWord);
            add(endWord);
        }};
        // 当前搜索过的层数
        int step = 1;
        // 开启BFS
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            // 从beginSet和endSet中选择当前数目小的进行下一层搜索,令节点数少的为beginSet
            if (beginSet.size() > endSet.size()) {
                Set<String> tmp = beginSet;
                beginSet = endSet;
                endSet = tmp;
            }
            // 装新一层的节点
            Set<String> newSet = new HashSet<>();
            // 开始搜索beginSet的每一个单词的下一层
            for (String s : beginSet) {
                // 弹出的节点(单词),以该节点为起点进行搜索
                char[] poll = s.toCharArray();
                // 遍历poll的每个字母
                for (int j = 0; j < poll.length; j++) {
                    // 记住原来的poll[j]处的字母以便还原
                    char origin = poll[j];
                    // 每个字母可改变范围为'a'-'z'
                    for (int k = 'a'; k <= 'z'; k++) {
                        // 原来的可以跳过
                        if (k == origin) {
                            continue;
                        }
                        // 替换poll[j]为k
                        poll[j] = (char) k;
                        // 生成新单词
                        String newWord = String.valueOf(poll);
                        // 若wordSet中能找到newWord
                        if (wordSet.contains(newWord)) {
                            // 若相遇了,直接返回搜索节点总个数,此时的路径最短
                            if (endSet.contains(newWord)) {
                                // 这里一般是搜了一层就+1,最后包含当前层搜的再+1,调整一下就可以
                                return step + 1;
                            }
                            // 没相遇且没被搜索过:继续搜索
                            if (!visited.contains(newWord)) {
                                // newWord加入新一层接节点
                                newSet.add(newWord);
                                // 标记搜索过
                                visited.add(newWord);
                            }
                        }
                    }
                    // 还原poll[j]并进行下一个字母的搜索(因为只能改变一个字母)
                    poll[j] = origin;
                }
            }
            // 一层节点搜索完毕,圈数+1
            step++;
            // beginSet取代newSet并进行下一轮搜索
            beginSet = newSet;
        }
        // 最后不能相遇,返回0
        return 0;
    }
}
```

```
HashSet is a set, e.g. {1,2,3,4,5}

HashMap is a key -> value (key to value) map, e.g. {a -> 1, b -> 2, c -> 2, d -> 1}

Notice in my example above that in the HashMap there must not be duplicate keys, but it may have duplicate values.

In the HashSet, there must be no duplicate elements.
```