# 212. Word Search II (欠)

Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
>(Solution 0 一步一步**优化的过程**，非常值得看
https://leetcode.cn/problems/word-search-ii/solution/tong-ge-lai-shua-ti-la-yi-ti-si-jie-zi-d-2igi/)

## Solution 0
```java
import java.util.ArrayList;
import java.util.List;

class Solution {

    // 上下左右移动的方向
    int[][] dirs = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public List<String> findWords(char[][] board, String[] words) {
        // 结果集
        List<String> resultList = new ArrayList<>();

        // 构建字典树
        TrieNode root = buildTrie(words);

        int m = board.length;
        int n = board[0].length;
        // 记录沿途遍历到的元素
        StringBuilder result = new StringBuilder();

        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                // 从每个元素开始遍历
                dfs(resultList, result, board, i, j, root, root);
            }
        }

        // 题目要求返回List
        return resultList;
    }

    private void dfs(List<String> resultList, StringBuilder result, char[][] board,
                     int i, int j, TrieNode root, TrieNode node) {
        // 判断越界，或者访问过，或者不在字典树中，直接返回
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length
                || board[i][j] == '#'
                || node == null || node.children[board[i][j] - 'a'] == null) {
            return;
        }

        // 记录当前字符
        char curr = board[i][j];
        result.append(curr);

        // 如果有结束字符，加入结果集中
        if (node.children[board[i][j] - 'a'].isEnd) {
            String word = result.toString();
            resultList.add(word);
            deleteWordFromTrie(root, word);
        }

        // 记录当前元素已访问
        board[i][j] = '#';

        // 按四个方向去遍历
        for (int[] dir : dirs) {
            dfs(resultList, result, board, i + dir[0], j + dir[1], root, node.children[curr - 'a']);
        }

        // 还原状态
        board[i][j] = curr;
        result.deleteCharAt(result.length() - 1);
    }

    private void deleteWordFromTrie(TrieNode root, String word) {
        // 删除并没有那么好搞，需要先找到最后一个字符，从下往上删除
        delete(root, word, 0);
    }

    // 返回true表示可以把沿途节点删除，返回false表示不能删除沿途节点
    private boolean delete(TrieNode prev, String word, int i) {
        if (i == word.length() - 1) {
            // 如果后面还有单词则不能直接删除，比如dog和dogs是在一条链上，删除dog的时候不能把整个链删除了
            TrieNode curr = prev.children[word.charAt(i) - 'a'];
            if (hasChildren(curr)) {
                curr.isEnd = false;
                return false;
            } else {
                prev.children[word.charAt(i) - 'a'] = null;
                return true;
            }
        } else {
            // 如果后面的说可以删除，并且当前节点不是单词节点，并且没有其它子节点了，那么删除之，否则返回false
            // 比如删除dogs的时候不能把dog删除了
            // 比如同时存在dog和dig两个单词，删除dog的时候返回到d的时候，这个d是不能删除的
            if (delete(prev.children[word.charAt(i) - 'a'], word, i + 1)
                    && !prev.children[word.charAt(i) - 'a'].isEnd
                    && !hasChildren(prev.children[word.charAt(i) - 'a'])) {
                prev.children[word.charAt(i) - 'a'] = null;
                return true;
            }
            return false;
        }
    }

    private boolean hasChildren(TrieNode curr) {
        for (TrieNode child : curr.children) {
            if (child != null) {
                return true;
            }
        }
        return false;
    }

    private TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String word : words) {
            char[] arr = word.toCharArray();
            TrieNode curr = root;
            for (char c : arr) {
                if (curr.children[c - 'a'] == null) {
                    curr.children[c - 'a'] = new TrieNode();
                }
                curr = curr.children[c - 'a'];
            }
            curr.isEnd = true;
        }
        return root;
    }

    class TrieNode {
        // 记录到这个节点是否是一个完整的单词
        boolean isEnd = false;
        // 孩子节点，题目说了都是小写字母，所以用数组，否则可以用HashMap替换
        TrieNode[] children = new TrieNode[26];
    }
}
```

## Solution 1 (Tries)
>我们可以使用 TrieTrie 结构进行建树，对于任意一个当前位置 (i, j)(i,j) 而言，只有在 TrieTrie 中存在往从字符 aa 到 bb 的边时，我们才在棋盘上搜索从 aa 到 bb 的相邻路径。

不了解 TrieTrie 的同学，可以看看 这里，里面写了两种实现 TrieTrie 的方式。

对于本题，我们可以使用「TrieNode」的方式进行建 TrieTrie。

因为 words 里最多有 10^410 
4
  个单词，每个单词长度最多为 1010，如果开成静态数组的话，不考虑共用行的问题，我们需要开一个大小为 10^5 * 2610 
5
 ∗26 的大数组，可能会有 TLE 或 MLE 的风险。
与此同时，我们需要将平时建 TrieNodeTrieNode 中的 isEnd 标记属性直接换成记录当前字符 s，这样我们在 DFS 的过程中则无须额外记录当前搜索字符串。

```java
class Solution {
    class TrieNode {
        String s;
        TrieNode[] tns = new TrieNode[26];
    }
    void insert(String s) {
        TrieNode p = root;
        for (int i = 0; i < s.length(); i++) {
            int u = s.charAt(i) - 'a';
            if (p.tns[u] == null) p.tns[u] = new TrieNode();
            p = p.tns[u];
        }
        p.s = s;
    }
    Set<String> set = new HashSet<>();
    char[][] board;
    int n, m;
    TrieNode root = new TrieNode();
    int[][] dirs = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
    boolean[][] vis = new boolean[15][15];
    public List<String> findWords(char[][] _board, String[] words) {
        board = _board;
        m = board.length; n = board[0].length;
        for (String w : words) insert(w);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int u = board[i][j] - 'a';
                if (root.tns[u] != null) {
                    vis[i][j] = true;
                    dfs(i, j, root.tns[u]);
                    vis[i][j] = false;
                }
            }
        }
        List<String> ans = new ArrayList<>();
        for (String s : set) ans.add(s);
        return ans;
    }
    void dfs(int i, int j, TrieNode node) {
        if (node.s != null) set.add(node.s);
        for (int[] d : dirs) {
            int dx = i + d[0], dy = j + d[1];
            if (dx < 0 || dx >= m || dy < 0 || dy >= n) continue;
            if (vis[dx][dy]) continue;
            int u = board[dx][dy] - 'a';
            if (node.tns[u] != null) {
                vis[dx][dy] = true;
                dfs(dx, dy, node.tns[u]);
                vis[dx][dy] = false;
            }
        }
    }
}

```

 ## Solution 2 (backtracking)
 >数据范围只有 1212，且 words 中出现的单词长度不会超过 1010，可以考虑使用「回溯算法」。
起始先将所有 words 出现的单词放到 Set 结构中，然后以 board 中的每个点作为起点进行爆搜（由于题目规定在一个单词中每个格子只能被使用一次，因此还需要一个 vis 数组来记录访问过的位置）：
如果当前爆搜到的字符串长度超过 1010，直接剪枝；
如果当前搜索到的字符串在 Set 中，则添加到答案（同时了防止下一次再搜索到该字符串，需要将该字符串从 Set 中移除）。

```java
class Solution {
    Set<String> set = new HashSet<>();
    List<String> ans = new ArrayList<>();
    char[][] board;
    int[][] dirs = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
    int n, m;
    boolean[][] vis = new boolean[15][15];
    public List<String> findWords(char[][] _board, String[] words) {
        board = _board;
        m = board.length; n = board[0].length;
        for (String w : words) set.add(w);
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                vis[i][j] = true;
                sb.append(board[i][j]);
                dfs(i, j, sb);
                vis[i][j] = false;
                sb.deleteCharAt(sb.length() - 1);
            }
        }
        return ans;
    }
    void dfs(int i, int j, StringBuilder sb) {
        if (sb.length() > 10) return ;
        if (set.contains(sb.toString())) {
            ans.add(sb.toString());
            set.remove(sb.toString());
        }
        for (int[] d : dirs) {
            int dx = i + d[0], dy = j + d[1];
            if (dx < 0 || dx >= m || dy < 0 || dy >= n) continue;
            if (vis[dx][dy]) continue;
            vis[dx][dy] = true;
            sb.append(board[dx][dy]);
            dfs(dx, dy, sb);
            vis[dx][dy] = false;
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}

```