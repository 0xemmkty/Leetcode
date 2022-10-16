# 286. Walls and Gates
You are given a m x n 2D grid initialized with these three possible values.

-1 - A wall or an obstacle.
0 - A gate.
INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.
```
Example: 

Given the 2D grid:

INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
After running your function, the 2D grid should be:

  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```

## Solution 1 (DFS)
```java
 public class Solution {
    public void wallsAndGates(int[][] rooms) {
        if (rooms == null || rooms.length == 0) {
            return;
        }
         
        int m = rooms.length;
        int n = rooms[0].length;
         
        boolean[][] visited = new boolean[m][n];
         
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rooms[i][j] == 0) {
                    wallsAndGatesHelper(i, j, 0, visited, rooms);
                }
            }
        }
    }
     
    private void wallsAndGatesHelper(int row, int col, int distance, boolean[][] visited, int[][] rooms) {
        int rows = rooms.length;
        int cols = rooms[0].length;
         
        if (row < 0 || row >= rows || col < 0 || col >= cols) {
            return;
        }
         
        // visited
        if (visited[row][col]) {
            return;
        }
         
        // Is wall?
        if (rooms[row][col] == -1) {
            return;
        }
         
        // Distance greater than current
        if (distance > rooms[row][col]) {
            return;
        }
         
         
        // Mark as visited
        visited[row][col] = true;
         
        if (distance < rooms[row][col]) {
            rooms[row][col] = distance;
        }
         
        // go up, down, left and right
        wallsAndGatesHelper(row - 1, col, distance + 1, visited, rooms);
        wallsAndGatesHelper(row + 1, col, distance + 1, visited, rooms);
        wallsAndGatesHelper(row, col - 1, distance + 1, visited, rooms);
        wallsAndGatesHelper(row, col + 1, distance + 1, visited, rooms);
         
        // Mark as unvisited
        visited[row][col] = false;
    }
}
// why use 0 to determind / start the DFS:
// 0 is a min distamce num, it will never be 
// change if it is 0
// other num (even the INF) will be change to the small one
// so just consider the -1 case
// other case will be solved in the later code
// good point!
// just easy question!
```
