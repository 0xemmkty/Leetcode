# 323. Number of Connected Components in an Undirected Graph 

>Given n nodes labeled from 0 to n - 1 and a **list of undirected edges** (each edge is a pair of nodes), write a function to find **the number of connected components** in an undirected graph.

>**Note**:
You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.


```
Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4 

Output: 2
```
pay attention to this example: 
``` 
Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
```
sol1 and sol2
## Solution 1 (UF)
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        int count = n;
        int[] parents = new int[n];
        for (int i = 0; i < n; i++) {
            parents[i] = i;
        }
        for (int[] edge : edges) {
            int p = find(parents, edge[0]);
            int q = find(parents, edge[1]);
            if (p != q) {
                parents[p] = q;
                count--;
            }
        }
        return count;
    }

    private int find(int[] parents, int i) {
        while (parents[i] != i) {
            parents[i] = parents[parents[i]]; // 路径压缩
            i = parents[i];
        }
        return i;
    }
}
// 时间O(V * E)

// 空间O(n)


```

## SOlution 2 (DFS)
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        int count = 0;
        List<List<Integer>> g = new ArrayList<>();
        boolean[] visited = new boolean[n];
        for (int i = 0; i < n; i++) {
            g.add(new ArrayList<>());
        }
        for (int[] e : edges) {
            g.get(e[0]).add(e[1]);
            g.get(e[1]).add(e[0]);
        }

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                count++;
                dfs(visited, i, g);
            }
        }
        return count;
    }

    private void dfs(boolean[] visited, int index, List<List<Integer>> g) {
        visited[index] = true;
        for (int i : g.get(index)) {
            if (!visited[i]) {
                dfs(visited, i, g);
            }
        }
    }
}
// 时间O(n^2)

// 空间O(n)
```

## Solution 3 (BFS)
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        int count = 0;
        List<List<Integer>> g = new ArrayList<>();
        boolean[] visited = new boolean[n];
        for (int i = 0; i < n; i++) {
            g.add(new ArrayList<>());
        }
        for (int[] e : edges) {
            g.get(e[0]).add(e[1]);
            g.get(e[1]).add(e[0]);
        }

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                count++;
                Queue<Integer> queue = new LinkedList<>();
                queue.offer(i);
                while (!queue.isEmpty()) {
                    int index = queue.poll();
                    visited[index] = true;
                    for (int next : g.get(index)) {
                        if (!visited[next]) {
                            queue.offer(next);
                        }
                    }
                }
            }
        }
        return count;
    }
}
// 时间O(n^2)

// 空间O(n)
```
