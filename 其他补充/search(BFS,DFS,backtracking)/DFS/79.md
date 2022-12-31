# 79. Word Search

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

## Solution 1
```java
public class Solution {

    private static final int[][] DIRECTIONS = {{-1, 0}, {0, -1}, {0, 1}, {1, 0}};
    private int rows;
    private int cols;
    private int len;
    private boolean[][] visited;
    private char[] charArray;
    private char[][] board;

    public boolean exist(char[][] board, String word) {
        rows = board.length;
        if (rows == 0) {
            return false;
        }
        cols = board[0].length;
        visited = new boolean[rows][cols];

        this.len = word.length();
        this.charArray = word.toCharArray();
        this.board = board;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (dfs(i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(int x, int y, int begin) {
        if (begin == len - 1) {
            return board[x][y] == charArray[begin];
        }
        if (board[x][y] == charArray[begin]) {
            visited[x][y] = true;
            for (int[] direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];
                if (inArea(newX, newY) && !visited[newX][newY]) {
                    if (dfs(newX, newY, begin + 1)) {
                        return true;
                    }
                }
            }
            visited[x][y] = false;
        }
        return false;
    }

    private boolean inArea(int x, int y) {
        return x >= 0 && x < rows && y >= 0 && y < cols;
    }
}

```

## Solution 2 (回溯+剪枝)
```java
class Solution3 {

    private boolean[][] used;
    private int row, col;
    private char[][] board;
    private char[] ws;
    private int[][] direction = new int[][] {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};

    public boolean exist(char[][] board, String word) {
        this.row = board.length;
        this.col = board[0].length;
        this.used = new boolean[row][col];
        for (boolean[] u : used) {
            Arrays.fill(u, false);
        }

        this.board = board;
        this.ws = word.toCharArray();

        for (int i = 0; i < row; ++i) {
            for (int j = 0; j < col; ++j) {
                if (board[i][j] == ws[0]) {
                    used[i][j] = true;
                    if (dfs(i, j, 1)) {
                        return true;
                    } else {
                        used[i][j] = false;
                    }
                }
            }
        }
        return false;
    }

    private boolean dfs(int i, int j, int depth) {
        if (depth == ws.length) {
            return true;
        }

        for (int[] d : direction) {
            int newX = i + d[0];
            int newY = j + d[1];
            // 剪枝
            if (! inArea(newX, newY)) {
                continue;
            }
            // 剪枝
            if (used[newX][newY]) {
                continue;
            }
            // 剪枝
            if (board[newX][newY] != ws[depth]) {
                continue;
            }

            used[newX][newY] = true;
            if (dfs(newX, newY, depth + 1)) {
                return true;
            } else {
                used[newX][newY] = false;
            }
        }
        return false;
    }

    private boolean inArea(int x, int y) {
        return x >= 0 && x < this.row && y >= 0 && y < this.col;
    }


    private void debug(List<int[]> list) {
        StringBuilder sb = new StringBuilder();
        for (int[] l : list) {
            sb.append("(" + l[0] + "," + l[1] + ")" + "\n");
        }
        System.out.println(sb.toString());
    }

    public static void main(String[] args) {
        char[][] board = new char[][] {
                {'A','B','C','E'},
                {'S','F','C','S'},
                {'A','D','E','E'}
        };
        String word = "ABCCED";
        boolean exist = new Solution3().exist(board, word);
        System.out.println(exist);
    }
}
```