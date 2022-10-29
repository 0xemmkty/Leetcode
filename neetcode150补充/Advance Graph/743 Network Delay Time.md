# 743. Network Delay Time (dai)

>You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.

>We will send a signal from a given node k. Return the minimum time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

https://leetcode.cn/problems/network-delay-time/solution/gong-shui-san-xie-yi-ti-wu-jie-wu-chong-oghpz/
来源：力扣（LeetCode）

## Solution 0 Floyd（邻接矩阵）

根据「基本分析」，我们可以使用复杂度O(n^3) 的「多源汇最短路」算法 Floyd 算法进行求解，同时使用「邻接矩阵」来进行存图。

此时计算量约为 106106，可以过。跑一遍 Floyd，可以得到「从任意起点出发，到达任意起点的最短距离」。然后从所有 w[k][x] 中取 max 即是「从 k 点出发，到其他点x的最短距离的最大值」。

时间复杂度:
O(n^3)
空间复杂度：
O(n^2)

```java
class Solution {
    int N = 110, M = 6010;
    // 邻接矩阵数组：w[a][b] = c 代表从 a 到 b 有权重为 c 的边
    int[][] w = new int[N][N];
    int INF = 0x3f3f3f3f;
    int n, k;
    public int networkDelayTime(int[][] ts, int _n, int _k) {
        n = _n; k = _k;
        // 初始化邻接矩阵
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                w[i][j] = w[j][i] = i == j ? 0 : INF;
            }
        }
        // 存图
        for (int[] t : ts) {
            int u = t[0], v = t[1], c = t[2];
            w[u][v] = c;
        }
        // 最短路
        floyd();
        // 遍历答案
        int ans = 0;
        for (int i = 1; i <= n; i++) {
            ans = Math.max(ans, w[k][i]);
        }
        return ans >= INF / 2 ? -1 : ans;
    }
    void floyd() {
        // floyd 基本流程为三层循环：
        // 枚举中转点 - 枚举起点 - 枚举终点 - 松弛操作        
        for (int p = 1; p <= n; p++) {
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n; j++) {
                    w[i][j] = Math.min(w[i][j], w[i][p] + w[p][j]);
                }
            }
        }
    }
}
```

## Solution 1
将边集数组转换成邻接矩阵
时间复杂度：
O(n^3)
空间复杂度：
O(n^2)
```java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        //创建带权网。
        int[][] edges=new int[n+1][n+1];
        for(int i=0; i<edges.length; i++)
            Arrays.fill(edges[i], Integer.MAX_VALUE);
        for(int i=0; i<times.length; i++)
            edges[times[i][0]][times[i][1]]=times[i][2];
        //根据给定的顶点初始化。
        boolean[] visited=new boolean[n+1];
        int[] minCost=new int[n+1];
        Arrays.fill(visited, false);
        Arrays.fill(minCost, Integer.MAX_VALUE);
        visited[k]=true;
        for(int i=1; i<minCost.length; i++)
            minCost[i]=edges[k][i];
        minCost[k]=0;
        //Dijkstra算法。
        int min, vertex=-1;
        for(int i=1; i<n; i++){
            min=Integer.MAX_VALUE;
            for(int j=1; j<minCost.length; j++){
                if(visited[j]==false && min>minCost[j]){
                    min=minCost[j];
                    vertex=j;
                }
            }
            if(min==Integer.MAX_VALUE)
                return -1;
            visited[vertex]=true;
            for(int m=1; m<edges[vertex].length; m++){
                if(visited[m]==false && edges[vertex][m]!=Integer.MAX_VALUE && edges[vertex][m]+min<minCost[m]){
                    minCost[m]=edges[vertex][m]+min;
                }
            }
        }
        //所需最长时间。
        int maxTime=-1;
        for(int i=1; i<minCost.length; i++)
            if(maxTime<minCost[i])
                maxTime=minCost[i];
        return maxTime;
    }
}
```

## Solution 2(堆优化 Dijkstra（邻接表）)
时间复杂度：O(mlogn+n)

空间复杂度：O(m)
```java
public class solution {
    // 图结点
    private int N = 110;
    private int M = 6010;

    // 邻接表
    // 存储与 结点v 直接相连的边编号集合(链表)的头结点(边编号)
    private int[] head = new int[N];

    // 存储某一条边指向的结点, 用于访问编号为idx的边所指向的结点
    private int[] edge = new int[M];

    // 由于以链表的形式存储边, 故可根据与 结点v 直接相连的边集合(链表)的头结点在 nextEdge数组 中找到下一条边
    // 存储 链表中边编号为idx的边指向/连接的下一条边
    // this.nextEdge[idx]: 保存边编号为idx边连接的下一条边
    private int[] nextEdge = new int[M];

    // 记录某条边的权重
    private int[] weight = new int[M];

    // 无穷大上界
    private int INF = 0x3f3f3f3f;

    // 结点数
    private int n;

    // 源点
    private int src;

    // 用于对边进行编号: 会自动初始化 赋0
    private int idx = 0;

    /**
     * 邻接表 加边操作
     * @param srcV: 源点
     * @param desV: 目标点
     * @param weight: 边权重
     */
    private void add(int srcV, int desV, int weight) {
        // 由源点(srcV)出发 经 编号为idx的边 指向 目标点(desV)
        // 新建一条编号为idx, 指向desV结点的边
        this.edge[idx] = desV;

        // 头插法
        // 1.新插入节点指向head的next；
        // 2. 断裂head原先（指向head的next）；
        // 3. head指向idx
        this.nextEdge[idx] = this.head[srcV];
        this.head[srcV] = idx;

        // 将编号为idx的边的权值 赋为 weight
        this.weight[idx] = weight;
        idx++;
    }


    /**
     * 遍历 结点src 与其相连的所有边
     * @param src
     */
    private void traversal(int src) {
        for (int idx = head[src]; idx != -1; idx = this.nextEdge[idx]) {
            int des = this.edge[idx];
            System.out.println("结点" + src + " -> 结点" + des);
            System.out.println("编号为" + idx + "的边, 权重为: " + weight[idx]);
        }
    }


    public int networkDelayTime(int[][] times, int n, int k) {
        this.n = n;
        this.src = k;

        Arrays.fill(head, -1);

        for (int[] time : times) {
            int src = time[0];
            int des = time[1];
            int weight = time[2];
            this.add(src, des, weight);
        }

        // 最短路径
        this.dijkstraOptimizedByHeap();

        int ans = 0;
        for (int i = 1; i <= n; i++) {
            ans = Math.max(ans, dist[i]);
        }

        return ans == INF ? -1 : ans;
    }


    // dist[v] = m 表示从[源点/起点] 到 结点v 的最短距离为 m
    private int[] dist = new int[N];

    // 记录结点的访问状态
    private boolean[] visited = new boolean[N];

    /**
     * Dijkstra算法: 单源最短路径算法
     *
     * 堆优化邻接表实现
     */
    private void dijkstraOptimizedByHeap() {
        Arrays.fill(dist, INF);
        Arrays.fill(visited, false);

        dist[src] = 0;

        // 升序排列, 最小堆
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[1] - b[1]);

        // new int[]{结点v, 与源点src的最短距离}
        queue.add(new int[]{this.src, 0});

        while (!queue.isEmpty()) {
            // 每次抛出与源点(src)最短距离最小的结点
            int[] poll = queue.poll();

            int v = poll[0];
            int distance = poll[1];

            // 已访问, 则跳过
            if (visited[v]) {
                continue;
            }

            // 将 结点v 置为已访问状态
            visited[v] = true;

            for (int idx = head[v]; idx != -1; idx = nextEdge[idx]) {
                int des = edge[idx];

                // 若源点(src) -> 结点v 的最短距离 + 结点v -> 结点des 的距离 < 源点(src) -> 结点des的最短距离
                if (dist[v] + weight[idx] < dist[des]) {
                    // 更新 结点des 的最短距离
                    dist[des] = dist[v] + weight[idx];
                    queue.add(new int[]{des, dist[des]});
                }
            }
        }
    }
    ```

## Solution 3
Bellman Ford（类 & 邻接表）

虽然题目规定了不存在「负权边」，但我们仍然可以使用可以在「负权图中求最短路」的 Bellman Ford 进行求解，该算法也是「单源最短路」算法，复杂度为 O(n∗m)。

通常为了确保 O(n∗m)，可以单独建一个类代表边，将所有边存入集合中，在 
n次松弛操作中直接对边集合进行遍历（代码见 

由于本题边的数量级大于点的数量级，因此也能够继续使用「邻接表」的方式进行边的遍历，遍历所有边的复杂度的下界为 

O(n)，上界可以确保不超过 

O(m)（代码见 
P。

```java
class Solution {
    class Edge {
        int a, b, c;
        Edge(int _a, int _b, int _c) {
            a = _a; b = _b; c = _c;
        }
    }
    int N = 110, M = 6010;
    // dist[x] = y 代表从「源点/起点」到 x 的最短距离为 y
    int[] dist = new int[N];
    int INF = 0x3f3f3f3f;
    int n, m, k;
    // 使用类进行存边
    List<Edge> es = new ArrayList<>();
    public int networkDelayTime(int[][] ts, int _n, int _k) {
        n = _n; k = _k;
        m = ts.length;
        // 存图
        for (int[] t : ts) {
            int u = t[0], v = t[1], c = t[2];
            es.add(new Edge(u, v, c));
        }
        // 最短路
        bf();
        // 遍历答案
        int ans = 0;
        for (int i = 1; i <= n; i++) {
            ans = Math.max(ans, dist[i]);
        }
        return ans > INF / 2 ? -1 : ans;
    }
    void bf() {
        // 起始先将所有的点标记为「距离为正无穷」
        Arrays.fill(dist, INF);
        // 只有起点最短距离为 0
        dist[k] = 0;
        // 迭代 n 次
        for (int p = 1; p <= n; p++) {
            int[] prev = dist.clone();
            // 每次都使用上一次迭代的结果，执行松弛操作
            for (Edge e : es) {
                int a = e.a, b = e.b, c = e.c;
                dist[b] = Math.min(dist[b], prev[a] + c);
            }
        }
    }
}
```

时间复杂度：
O(n∗m)
空间复杂度：
O(m)

## Solution 4 SPFA（邻接表）

SPFA 是对 Bellman Ford 的优化实现，可以使用队列进行优化，也可以使用栈进行优化。

通常情况下复杂度为 
O(k∗m)，
k
k 一般为 
4
4 到 
5
5，最坏情况下仍为 
O
(
n
∗
m
)
O(n∗m)，当数据为网格图时，复杂度会从 
O
(
k
∗
m
)
O(k∗m) 退化为 
O
(
n
∗
m
)
O(n∗m)。

```java
class Solution {
    int N = 110, M = 6010;
    // 邻接表
    int[] he = new int[N], e = new int[M], ne = new int[M], w = new int[M];
    // dist[x] = y 代表从「源点/起点」到 x 的最短距离为 y
    int[] dist = new int[N];
    // 记录哪一个点「已在队列」中
    boolean[] vis = new boolean[N];
    int INF = 0x3f3f3f3f;
    int n, k, idx;
    void add(int a, int b, int c) {
        e[idx] = b;
        ne[idx] = he[a];
        he[a] = idx;
        w[idx] = c;
        idx++;
    }
    public int networkDelayTime(int[][] ts, int _n, int _k) {
        n = _n; k = _k;
        // 初始化链表头
        Arrays.fill(he, -1);
        // 存图
        for (int[] t : ts) {
            int u = t[0], v = t[1], c = t[2];
            add(u, v, c);
        }
        // 最短路
        spfa();
        // 遍历答案
        int ans = 0;
        for (int i = 1; i <= n; i++) {
            ans = Math.max(ans, dist[i]);
        }
        return ans > INF / 2 ? -1 : ans;
    }
    void spfa() {
        // 起始先将所有的点标记为「未入队」和「距离为正无穷」
        Arrays.fill(vis, false);
        Arrays.fill(dist, INF);
        // 只有起点最短距离为 0
        dist[k] = 0;
        // 使用「双端队列」存储，存储的是点编号
        Deque<Integer> d = new ArrayDeque<>();
        // 将「源点/起点」进行入队，并标记「已入队」
        d.addLast(k);
        vis[k] = true;
        while (!d.isEmpty()) {
            // 每次从「双端队列」中取出，并标记「未入队」
            int poll = d.pollFirst();
            vis[poll] = false;
            // 尝试使用该点，更新其他点的最短距离
            // 如果更新的点，本身「未入队」则加入队列中，并标记「已入队」
            for (int i = he[poll]; i != -1; i = ne[i]) {
                int j = e[i];
                if (dist[j] > dist[poll] + w[i]) {
                    dist[j] = dist[poll] + w[i];
                    if (vis[j]) continue;
                    d.addLast(j);
                    vis[j] = true;
                }
            }
        }
    }
}
```
