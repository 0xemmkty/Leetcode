##  269.Alien Dictionary 另类字典
 

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of non-empty words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language.

Example 1:

Input:
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]

Output: 
"wertf"

>wrt>wrf =>t>f, wrf>er=>w>e，er>et=>r>t，ett>rftt=>e>r， 因此可得出w>e>r>t>f。也就是说给定了一系列顺序，要求返回根据顺序遍历的结果，即拓扑排序。先根据input的单词生成单个字符之间的顺序对，如t>f, r>t等。再对字符顺序对做拓扑排序。

Example 2：

输入:
[
  "z",
  "x",
  "z"
] 

输出: "" 

解释: 非法排序， 返回"".

>**建议直接看solution 3**

### Solution 1
>这些就是有向图的边，对于有向图中的每个结点，计算其入度，然后从入度为0的结点开始 BFS 遍历这个有向图，然后将遍历路径保存下来返回即可。下面来看具体的做法：

根据之前讲解，需用 TreeSet 来保存这些 pair，还需要一个 HashSet 来保存所有出现过的字母，需要一个一维数组 in 来保存每个字母的入度，另外还要一个 queue 来辅助拓扑遍历，我们先遍历单词集，把所有字母先存入 HashSet，然后我们每两个相邻的单词比较，找出顺序 pair，然后根据这些 pair 来赋度，把 HashSet 中入度为0的字母都排入 queue 中，然后开始遍历，如果字母在 TreeSet 中存在，则将其 pair 中对应的字母的入度减1，若此时入度减为0了，则将对应的字母排入 queue 中并且加入结果 res 中，直到遍历完成，看结果 res 和 ch 中的元素个数是否相同，若不相同则说明可能有环存在，返回空字符串，参见代码如下：

```java
class Solution {
public:
    string alienOrder(vector<string>& words) {
        set<pair<char, char>> st;
        unordered_set<char> ch;
        vector<int> in(256);
        queue<char> q;
        string res;
        for (auto a : words) ch.insert(a.begin(), a.end());
        for (int i = 0; i < (int)words.size() - 1; ++i) {
            int mn = min(words[i].size(), words[i + 1].size()), j = 0;
            for (; j < mn; ++j) {
                if (words[i][j] != words[i + 1][j]) {
                    st.insert(make_pair(words[i][j], words[i + 1][j]));
                    break;
                }
            }
            if (j == mn && words[i].size() > words[i + 1].size()) return "";
        }
        for (auto a : st) ++in[a.second];
        for (auto a : ch) {
            if (in[a] == 0) {
                q.push(a);
                res += a;
            } 
        }
        while (!q.empty()) {
            char c = q.front(); q.pop();
            for (auto a : st) {
                if (a.first == c) {
                    --in[a.second];
                    if (in[a.second] == 0) {
                        q.push(a.second);
                        res += a.second;
                    }
                }
            }
        }
        return res.size() == ch.size() ? res : "";
    }
};
```

## Solution 2
>下面来看一种 DFS 的解法，思路和 BFS 的很类似，需要建立一个二维的 bool 数组g，为了节省空间，不必像上面的解法中一样使用一个 HashSet 来记录所有出现过的字母，可以直接用这个二维数组来保存这个信息，只要 g[i][i] = true，即表示位置为i的字母存在。同时，这个二维数组还可以保存顺序对儿的信息，只要 g[i][j] = true，就知道位置为i的字母顺序在位置为j的字母前面。找顺序对儿的方法跟上面的解法完全相同，之后就可以进行 DFS 遍历了。由于 DFS 遍历需要标记遍历结点，那么就用一个 visited 数组，由于是深度优先的遍历，并不需要一定要从入度为0的结点开始遍历，而是从任意一个结点开始都可以，DFS 会遍历到出度为0的结点为止，加入结果 res，然后回溯加上整条路径到结果 res 即可，参见代码如下：

```java
class Solution {
public:
    string alienOrder(vector<string>& words) {
        vector<vector<bool>> g(26, vector<bool>(26));
        vector<bool> visited(26);
        string res;
        for (string word : words) {
            for (char c : word) {
                g[c - 'a'][c - 'a'] = true;
            }
        }
        for (int i = 0; i < (int)words.size() - 1; ++i) {
            int mn = min(words[i].size(), words[i + 1].size()), j = 0;
            for (; j < mn; ++j) {
                if (words[i][j] != words[i + 1][j]) {
                    g[words[i][j] - 'a'][words[i + 1][j] - 'a'] = true;
                    break;
                }
            }
            if (j == mn && words[i].size() > words[i + 1].size()) return "";
        }
        for (int i = 0; i < 26; ++i) {
            if (!dfs(g, i, visited, res)) return "";
        }
        return res;
    }
    bool dfs(vector<vector<bool>>& g, int idx, vector<bool>& visited, string& res) {
        if (!g[idx][idx]) return true;
        visited[idx] = true;
        for (int i = 0; i < 26; ++i) {
            if (i == idx || !g[i][idx]) continue;
            if (visited[i]) return false;
            if (!dfs(g, i, visited, res)) return false;
        }
        visited[idx] = false;
        g[idx][idx] = false;
        res += 'a' + idx;
        return true;
    }
};
```

https://www.cnblogs.com/grandyang/p/5250200.html

Solution 3:

本题相对选修课那题比较稍微复杂一些，复杂的地方在于题目中并没有给出”先修课”(排在当前字母前面字母)列表，这需要我们自行构建出该关系图，构建后利用拓扑排序解题即可。

构建关系图时，我们要利用题目给出的words数组，由于数组中单词已经按照字典顺序排序好，因此，我们可以对相邻的两两单词进行比较，从两个单词的第一位开始对比，如果字符相同就移动到下一位，如果不同，前面单词的当前字母应该排在后一单词当前字母的前面，也就是说前字母是后字母的”先修课”。比如ab和ac，第一位两个a相同，继续看第二位，b和c不同，因为b所在的单词字典顺序靠前，因此b应排在c的前面。我们利用一个Map保存统计结果，key为相对靠前的字母，value为所有排在该字母后方的字母的集合。

map建立好之后，利用拓扑排序思想解题即可。
```java

// 用于统计排在每种字母后面的所有字母
Map<Character, List<Character>> map = new HashMap<>();
// 拓扑排序用的访问数组
int[] visited = new int[26];
// 用于统计words中存在哪些字母
boolean[] has = new boolean[26];
public String alienOrder(String[] words) {
    // 统计words中存在哪些字母
    for(int i=0;i<words.length;i++){
        String current=words[i];
        for(int j=0;j<current.length();j++){
            has[current.charAt(j)-'a']=true;
        }
    }
    // 相邻2单词比较，统计排在每种字母后面的所有字母
    for(int i=1;i<words.length;i++){
        // 前单词
        String pre = words[i-1];
        // 当前单词
        String current=words[i];
        // 单词下标
        int index=0;
        // 比较2单词同一下标
        while(index<pre.length() && index<current.length()){
            // 前单词当前字符
            char p = pre.charAt(index);
            // 当前单词当前字符
            char c = current.charAt(index);
            // 2字符不同
            if(p!=c){
                // 将当前字母放入前字母的后续列表中
                List<Character> l=map.getOrDefault(p,new ArrayList<>());
                l.add(c);
                map.put(p, l);
                break;
            }
            index++;
        }
    }
    // 返回结果
    String res="";
    // 循环dfs每种字符
    for(int i=0;i<26;i++){
        // 如果该字母没有出现过，跳过
        if(!has[i]) continue;
        // 如果存在非法排序，返回空
        if(!dfs((char)(i+'a'))) return res;
    }
    // 因为拓扑排序是反向遍历，所以将结果倒序打印出来。
    for(int i=resList.size()-1;i>=0;i--){
        res+=resList.get(i);
    }
    return res;
}
List<Character> resList = new ArrayList<>();
// 拓扑排序（dfs）
boolean dfs(char c){
    if(visited[c-'a']==1) return false;
    if(visited[c-'a']==2) return true;
    visited[c-'a']=1;
    List<Character> list = map.get(c);
    if(list!=null) {
        for(Character next : list){
            if(!dfs(next)) return false;
        }
    }
    visited[c-'a']=2;
    resList.add(c);
    return true;
}
//Review the topic, we know that illegal sorting
//is like a "ring"
//so what we need to do is : when meet ring, reture ""
//(in the answer we return res, this is beause at first res is "")
//let begin our Problem solving method: use map to store
//current.charAt(j)-'a'
//(key, all alphabet behind key[use list to store]);
//int visited and boolean has;
//use double for loop to mark all appear alphabet[let has be true];
//find current string and pre string, use index to compare
//is cur[index] != pre[index]
//let arraylist.add(cur)
//map.getOrDefault
//(char)(i+'a')
//dfs result: 1.not appear--skip; 
//2. illegel sort--return ""
//3. correct answer--remember to reverse list
// dfs method: boolean method; use tow number to control



```
本题解法执行时间为2ms。
 