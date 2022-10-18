# 994. Rotting Oranges

>You are given an m x n grid where each cell can have one of three values:

>0 representing an empty cell,
1 representing a fresh orange, or
2 representing a rotten orange.
Every minute, **any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten**.

>Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

## solution 1 (BFS)
before write this question, it is important to think why we use BFS instead of DFS?
what the difference?
one in queue, and look at its around.

```java
public int orangesRotting(int[][] grid) {
        int M = grid.length;
        int N = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int[][] dir = { {-1,0},{1,0},{0,-1},{0,1} };//四个方向移动

        int count = 0; // count 表示新鲜橘子的数量
        for (int r = 0; r < M; r++) {
            for (int c = 0; c < N; c++) {
                if (grid[r][c] == 1) {
                    count++;
                } else if (grid[r][c] == 2) {
                    queue.add(new int[]{r, c});//腐烂的入队
                }
            }
        }

        int round = 0; // round 表示分钟数
        while (count > 0 && !queue.isEmpty()) {//注意条件 count不能少，否则会多计算
            round++;
            int n = queue.size();
            for(int i = 0; i < n; i++) {
                int[] tmp = queue.poll();
                for(int k = 0; k < 4; k++) {
                    int cr = tmp[0] + dir[k][0];
                    int cc = tmp[1] + dir[k][1];
                    if(cr >= 0 && cr < M && cc >= 0 && cc < N && grid[cr][cc] == 1) {
                        grid[cr][cc] = 2;//开始腐烂
                        count--;
                        queue.add(new int[]{cr, cc});//添加新元素
                    }
                }
            }
        
        }
        if (count > 0) {
            return -1;
        } else {
            return round;
        }
    }
    ```
    

