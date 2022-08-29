# LEETCODE 261. GRAPH VALID TREE 无向图成环问题

题目大意
Given n nodes labeled from 0 to n-1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

Example 1:

Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]
Output: true
Example 2:

Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
Output: false
Note: you can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0,1] is the same as [1,0] and thus will not appear together in edges.


是否成环问题
这道题可以联系 Leetcode 207. course schedule 一同分析，当我们把题意扯开，他们的核心都是判断一个图是有环图，还是无环图。
而这两个问题的核心区别在于向的问题，261 Graph Valid Tree 是判断无向图是否有环，而207 course schedule 是判断有向图是否有环。
我把这个类型的规律总结如下，
如果题目给出向前依赖关系，即给定a依赖于b,c,d,e, 则DFS更适合；
如果题目给出向后依赖关系，即给定a支持b,c,d,e,则BFS更适合；
如果题目给出无向图，则BFS与DFS大致等价

给定从 0 到 n-1 标号的 n 个结点，和一个无向边列表（每条边以结点对来表示），请编写一个函数用来判断这些边是否能够形成一个合法有效的树结构。

注意：你可以假定边列表 edges 中不会出现重复的边。由于所有的边是无向边，边 [0,1] 和边 [1,0] 是相同的，因此不会同时出现在边列表 edges 中。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/graph-valid-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

这道题属于图和树的综合题。给的input是节点的个数n和一些边edges，请你判断能否组成一棵树。有一个概念需要复习，树和图的区别在于图是可以有环 (cycle) 的，树不能有环；同时图的节点有可能不会连在一片，但是树的所有节点必定都是相连成一片的。对于这道题，有一些corner case需要特判，如果只有一个节点的话，那么就一定没有edges；因为题设说了不会给额外的边，所以如果边的数量 != 节点数量 - 1的话，一定不是树。

照着这个思路，我还是提供三种做法，BFS, DFS和Union Find。三种做法的时间空间复杂度都一样，union find在运行时间上可以通过路径压缩变得更快。
## Solution 1 BFS

时间O(V * E)

空间O(n)
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        // create graph
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        for (int i = 0; i < edges.length; i++) {
            graph.get(edges[i][0]).add(edges[i][1]);
            graph.get(edges[i][1]).add(edges[i][0]);
        }

        boolean[] visited = new boolean[n];
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(0);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            if (visited[cur] == true) {
                return false;
            }
            visited[cur] = true;
            for (int nei : graph.get(cur)) {
                if (!visited[nei]) {
                    queue.offer(nei);
                }
            }
        }

        for (int i = 0; i < n; i++) {
            if (visited[i] == false) {
                return false;
            }
        }
        return true;
    }
}
```
## Solution 2 DFS

时间O(V * E)

空间O(n)
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        // create graph
        List<List<Integer>> g = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            g.add(new ArrayList<>());
        }
        for (int[] e : edges) {
            g.get(e[0]).add(e[1]);
            g.get(e[1]).add(e[0]);
        }

        HashSet<Integer> visited = new HashSet<>();
        visited.add(0);
        boolean res = helper(g, visited, 0, -1);
        if (res == false) {
            return false;
        }
        return visited.size() == n ? true : false;
    }

    private boolean helper(List<List<Integer>> g, HashSet<Integer> visited, int cur, int parent) {
        List<Integer> neighbors = g.get(cur);
        for (int nei : neighbors) {
            if (nei == parent) {
                continue;
            }
            // cycle
            if (visited.contains(nei)) {
                return false;
            }
            visited.add(nei);
            boolean res = helper(g, visited, nei, cur);
            if (res == false) {
                return false;
            }
        }
        return true;
    }
}
```
## Solution 3  (Union Find)
时间O(V * E)

空间O(n)
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        // corner case
        if (n == 1 && edges.length == 0) {
            return true;
        }
        if (n < 1 || edges == null || edges.length != n - 1) {
            return false;
        }

        // normal case
        int[] roots = new int[n];
        for (int i = 0; i < n; i++) {
            roots[i] = -1;
        }

        for (int[] pair : edges) {
            int x = find(roots, pair[0]);
            int y = find(roots, pair[1]);
            if (x == y) {
                return false;
            }
            roots[x] = y;
        }
        return true;
    }

    private int find(int[] roots, int i) {
        while (roots[i] != -1) {
            i = roots[i];
        }
        return i;
    }
}
```

## Solution 4
BFS 实现
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        if (n==0||n==1) return true;
        if (edges==null||edges.length==0) return false;
        List<Integer>[] graph = (ArrayList<Integer>[]) new ArrayList[n];
        for (int[] edge:edges){
            if (graph[edge[0]]==null) graph[edge[0]] = new ArrayList<>();
            if (graph[edge[1]]==null) graph[edge[1]] = new ArrayList<>();
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        int root = edges[0][0];
        List<Integer> queue = new ArrayList<>();
        HashSet<Integer> visited = new HashSet<>();
        queue.add(root);
        while (!queue.isEmpty()){
            int number = queue.size();
            for (int i=0;i<number;i++){
                int node = queue.remove(0);
                if (visited.contains(node)) return false;
                visited.add(node);
                queue.addAll(graph[node]);
                for (int dest:graph[node]){
                    graph[dest].remove((Integer)node);
                }
            }
        }
        return visited.size()==n;
    }
}
```

## Solution 5
DFS实现
```java
class Solution {

    private HashSet<Integer> visited;
    private int n;
    private int[][] graph;

    private boolean dfs(int node){
        for (int dest=0;dest<n;dest++){
            if (graph[node][dest] == 0) continue;
            if (visited.contains(dest)) return false;
            visited.add(dest);
            graph[node][dest] = 0;
            graph[dest][node] = 0;
            if (!dfs(dest)) return false;
        }
        return true;
    }

    public boolean validTree(int n, int[][] edges) {

        if (n==0||n==1) return true;
        if (edges==null || edges.length==0) return false;
        this.n = n;
        graph = new int[n][n];
        visited = new HashSet<>();
        for (int[] edge:edges){
            graph[edge[0]][edge[1]] = 1;
            graph[edge[1]][edge[0]] = 1;
        }
        visited.add(edges[0][0]);
        int root = edges[0][0];
        return dfs(root) && visited.size()==n;
    }
}
```