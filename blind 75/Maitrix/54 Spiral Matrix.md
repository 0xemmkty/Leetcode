# 54. Spiral Matrix

Given an m x n matrix, return all elements of the matrix in spiral order.
## Solution 1
时间复杂度：O(mn)，其中 m 和 n 分别是输入矩阵的行数和列数。矩阵中的每个元素都要被访问一次。

空间复杂度：O(1)。除了输出数组以外，空间复杂度是常数。


```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> order = new ArrayList<>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return order;
        }

        int m = matrix.length;
        int n = matrix[0].length;
        int l = 0, r = n - 1, t = 0, b = m - 1;

        int num = 1, tar = m * n;
        while(num <= tar){
            for(int i = l; i <= r; i++) {
                num ++;
                order.add(matrix[t][i]);
            }
            if (t == b) {
                break;
            }
            t++;
            for(int i = t; i <= b; i++) {
                num ++;
                order.add(matrix[i][r]);
            }
            if (l == r) {
                break;
            }
            r--;
            for(int i = r; i >= l; i--) {
                num ++;
                order.add(matrix[b][i]);
            }
            b--;
            for(int i = b; i >= t; i--) {
                num ++;
                order.add(matrix[i][l]);
            }
            l++;
        }
        return order;
    }
}
```

## Solution 2 
方法一：模拟
可以模拟螺旋矩阵的路径。初始位置是矩阵的左上角，初始方向是向右，当路径超出界限或者进入之前访问过的位置时，顺时针旋转，进入下一个方向。

判断路径是否进入之前访问过的位置需要使用一个与输入矩阵大小相同的辅助矩阵 \textit{visited}visited，其中的每个元素表示该位置是否被访问过。当一个元素被访问时，将 \textit{visited}visited 中的对应位置的元素设为已访问。

如何判断路径是否结束？由于矩阵中的每个元素都被访问一次，因此路径的长度即为矩阵中的元素数量，当路径的长度达到矩阵中的元素数量时即为完整路径，将该路径返回。
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int[] dx = {0, 1, 0, -1}, dy = {1, 0, -1, 0};
        int n = matrix.length, m = matrix[0].length;
        List<Integer> res = new ArrayList<>();
        for(int i = 0, j = 0, d = 0, k = 0; k < n * m; k++){
            res.add(matrix[i][j]);
            matrix[i][j] = 101;
            int a = i + dx[d], b = j + dy[d];
            if(a < 0 || a >= n || b < 0 || b >= m || matrix[a][b] == 101){
                d = (d + 1) % 4;
                a = i + dx[d]; b = j + dy[d];
            }
            i = a; j = b;
        }
        return res;
    }
}
```

```Java

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> order = new ArrayList<Integer>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return order;
        }
        int rows = matrix.length, columns = matrix[0].length;
        boolean[][] visited = new boolean[rows][columns];
        int total = rows * columns;
        int row = 0, column = 0;
        int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        int directionIndex = 0;
        for (int i = 0; i < total; i++) {
            order.add(matrix[row][column]);
            visited[row][column] = true;
            int nextRow = row + directions[directionIndex][0], nextColumn = column + directions[directionIndex][1];
            if (nextRow < 0 || nextRow >= rows || nextColumn < 0 || nextColumn >= columns || visited[nextRow][nextColumn]) {
                directionIndex = (directionIndex + 1) % 4;
            }
            row += directions[directionIndex][0];
            column += directions[directionIndex][1];
        }
        return order;
    }
}
```
复杂度分析

时间复杂度：O(mn)O(mn)，其中 mm 和 nn 分别是输入矩阵的行数和列数。矩阵中的每个元素都要被访问一次。

空间复杂度：O(mn)O(mn)。需要创建一个大小为 m \times nm×n 的矩阵 \textit{visited}visited 记录每个位置是否被访问过。


```java
class Solution {
    int n,m;
    List<Integer> ans = new LinkedList<>();
    //                          右、    下、    左、    上
    int[][] dir = new int[][]{{0, 1},{1, 0},{0, -1},{-1, 0}};
    boolean[][] st;
    public List<Integer> spiralOrder(int[][] matrix) {
        n = matrix.length;
        m = matrix[0].length;
        st = new boolean[n][m];

        dfs(0, 0, 0, matrix);
        return ans;
    }

    public void dfs(int x, int y, int u, int[][] matrix){
        if(x >= n || x < 0 || y >= m || y < 0 || st[x][y]) return;
        
        st[x][y] = true;
        ans.add(matrix[x][y]);
        // 如果走的通，一直往u方向走
        int a = x + dir[u][0], b = y + dir[u][1];
        dfs(a, b, u, matrix);
        
        // 换下一个方向
        u = (u + 1) % 4; 
        a = x + dir[u][0];
        b = y + dir[u][1];
        dfs(a, b, u, matrix);

    }
}
```