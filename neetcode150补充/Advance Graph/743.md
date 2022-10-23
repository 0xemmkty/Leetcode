# 743. Network Delay Time

>You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.

>We will send a signal from a given node k. Return the minimum time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

## Solution 1
将边集数组转换成邻接矩阵
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