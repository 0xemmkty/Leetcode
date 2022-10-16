# 261 Graph Valid Tree (premium)
```
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), 
write a function to check whether these edges make up a valid tree.
For example:
Given n = 5 and edges = [[0, 1], [0, 2], [0, 3], [1, 4]], return true.
Given n = 5 and edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]], return false.
Hint:
Given n = 5 and edges = [[0, 1], [1, 2], [3, 4]], what should your return? Is this case a valid tree?
According to the definition of tree on Wikipedia: “a tree is an undirected graph 
in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”
Note: you can assume that no duplicate edges will appear in edges. 
Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together inedges.
```
```
这道题属于图和树的综合题。给的input是节点的个数n和一些边edges，请你判断能否组成一棵树。有一个概念需要复习，树和图的区别在于图是可以有环 (cycle) 的，树不能有环；同时图的节点有可能不会连在一片，但是树的所有节点必定都是相连成一片的。对于这道题，有一些corner case需要特判，如果只有一个节点的话，那么就一定没有edges；因为题设说了不会给额外的边，所以如果边的数量 != 节点数量 - 1的话，一定不是树。

照着这个思路，我还是提供三种做法，BFS, DFS和Union Find。三种做法的时间空间复杂度都一样，union find在运行时间上可以通过路径压缩变得更快。
```
## Solution 1 (UF)
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
// 时间O(V * E)

// 空间O(n)
```

## Solution 2
//DFS
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
//时间O(V * E)

//空间O(n)
```

other version
```java
public boolean validTree(int n, int[][] edges) {
    HashMap<Integer, ArrayList<Integer>> map = new HashMap<Integer, ArrayList<Integer>>();
    for(int i=0; i<n; i++){
        ArrayList<Integer> list = new ArrayList<Integer>();
        map.put(i, list);
    }
 
    for(int[] edge: edges){
        map.get(edge[0]).add(edge[1]);
        map.get(edge[1]).add(edge[0]);
    }
 
    boolean[] visited = new boolean[n];
 
    if(!helper(0, -1, map, visited))
        return false;
 
    for(boolean b: visited){
        if(!b)
            return false;
    }
 
    return true;
}
 
public boolean helper(int curr, int parent, 
                      HashMap<Integer, ArrayList<Integer>> map, boolean[] visited){
    if(visited[curr])
        return false;
 
    visited[curr] = true;
 
    for(int i: map.get(curr)){
        if(i!=parent && !helper(i, curr, map, visited)){
            return false;
        }
    }   
 
    return true;
}
```

## Solution 3
//BFS
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
other version
```java
public boolean validTree(int n, int[][] edges) {
        ArrayList<ArrayList<Integer>> list = new ArrayList<>();
        for(int i=0; i<n; i++){
            list.add(new ArrayList<>());
        }
 
        //build the graph
        for(int[] edge: edges){
            int a = edge[0];
            int b = edge[1];
 
            list.get(a).add(b);
            list.get(b).add(a);
        }
 
        //use queue to traverse the graph
        HashSet<Integer> visited = new HashSet<>();
        LinkedList<Integer> q = new LinkedList<>();
        q.offer(0);
 
        while(!q.isEmpty()){
            int head = q.poll();
 
            if(visited.contains(head)){
                return false;
            }
 
            visited.add(head);
 
            ArrayList<Integer> vList = list.get(head);
            for(int v: vList){
                if(!visited.contains(v)){
                    q.offer(v); 
                }     
            }
        }
 
        if(visited.size()<n){
            return false;
        }
 
        return true;
    }
    // 时间O(V * E)

    // 空间O(n)
```
