# 684. Redundant Connection

>In this problem, a tree is an undirected graph that is connected and has no cycles.

>You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph.

>Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are **multiple answers**, return the answer that **occurs last in the input**.
```
示例 2：



输入: [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
解释: 给定的无向图为:
5 - 1 - 2
    |   |
    4 - 3
```

## Solution 1 （UF）
```
方法：并查集
在一棵树中，边的数量比节点的数量少 1。如果一棵树有 N 个节点，则这棵树有 N - 1 条边。这道题中的图在树的基础上多了一条附加的边，因此边的数量也是 N。

树是一个连通且无环的无向图，在树中多了一条附加的边之后就会出现环，因此附加的边即为导致环出现的边。

可以通过并查集寻找附加的边。初始时，每个节点都属于不同的连通分量。遍历每一条边，判断这条边连接的两个顶点是否属于相同的连通分量。

如果两个顶点属于不同的连通分量，则说明在遍历到当前的边之前，这两个顶点之间不连通，因此当前的边不会导致环出现，合并这两个顶点的连通分量。
如果两个顶点属于相同的连通分量，则说明在遍历到当前的边之前，这两个顶点之间已经连通，因此当前的边导致环出现，为附加的边，将当前的边作为答案返回。
```
```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        int nodesCount = edges.length;
        int[] parent = new int[nodesCount + 1];
        for (int i = 1; i <= nodesCount; i++) {
            parent[i] = i;
        }
        for (int i = 0; i < nodesCount; i++) {
            int[] edge = edges[i];
            int node1 = edge[0], node2 = edge[1];
            if (find(parent, node1) != find(parent, node2)) {
                union(parent, node1, node2);
            } else {
                return edge;
            }
        }
        return new int[0];
    }
​
    public void union(int[] parent, int index1, int index2) {
        parent[find(parent, index1)] = find(parent, index2);
    }
​
    public int find(int[] parent, int index) {
        if (parent[index] != index) {
            parent[index] = find(parent, parent[index]);
        }
        return parent[index];
    }
}

```
```
复杂度分析

时间复杂度：O(NlogN) ，其中N 是图中的节点个数。需要遍历图中的 N 条边，对于每条边，需要对两个节点查找祖先，如果两个节点的祖先不同则需要进行合并，
需要进行 2次查找和最多 1 次合并。一共需要进行  2N次查找和最多 N 次合并，因此总时间复杂度是O(2NlogN) = O(NlogN)。
这里的并查集使用了路径压缩，但是没有使用按秩合并，最坏情况下的时间复杂度是 O(NlogN) ，平均情况下的时间复杂度依然是  ，
其中  为阿克曼函数的反函数，  可以认为是一个很小的常数。

空间复杂度：O(N)  ，其中 N 是图中的节点个数。使用数组 parent 记录每个节点的祖先。
```

## Solution 2 (DFS)
```java
class Solution {
    Set<Integer> seen = new HashSet();
    int MAX_EDGE_VAL = 1000;

    public int[] findRedundantConnection(int[][] edges) {
        ArrayList<Integer>[] graph = new ArrayList[MAX_EDGE_VAL + 1];
        for (int i = 0; i <= MAX_EDGE_VAL; i++) {
            graph[i] = new ArrayList();
        }

        for (int[] edge: edges) {
            seen.clear();
            if (!graph[edge[0]].isEmpty() && !graph[edge[1]].isEmpty() &&
                    dfs(graph, edge[0], edge[1])) {
                return edge;
            }
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        throw new AssertionError();
    }
    public boolean dfs(ArrayList<Integer>[] graph, int source, int target) {
        if (!seen.contains(source)) {
            seen.add(source);
            if (source == target) return true;
            for (int nei: graph[source]) {
                if (dfs(graph, nei, target)) return true;
            }
        }
        return false;
    }
}
```

Complexity Analysis

**Time Complexity**: O(N^2) where Nis the number of vertices (and also the number of edges) in the graph. In the worst case, for every edge we include, we have to search every previously-occurring edge of the graph.

**Space Complexity**: O(N). The current construction of the graph has at most NN nodes.

