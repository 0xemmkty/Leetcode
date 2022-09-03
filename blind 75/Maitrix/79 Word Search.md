# 79. Word Search

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

## Solution (回溯)
```java
public class Solution {

    private boolean[][] marked;

    //        x-1,y
    // x,y-1  x,y    x,y+1
    //        x+1,y
    private int[][] direction = {{-1, 0}, {0, -1}, {0, 1}, {1, 0}};
    // 盘面上有多少行
    private int m;
    // 盘面上有多少列
    private int n;
    private String word;
    private char[][] board;

    public boolean exist(char[][] board, String word) {
        m = board.length;
        if (m == 0) {
            return false;
        }
        n = board[0].length;
        marked = new boolean[m][n];
        this.word = word;
        this.board = board;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dfs(i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(int i, int j, int start) {
        if (start == word.length() - 1) {
            return board[i][j] == word.charAt(start);
        }
        if (board[i][j] == word.charAt(start)) {
            marked[i][j] = true;
            for (int k = 0; k < 4; k++) {
                int newX = i + direction[k][0];
                int newY = j + direction[k][1];
                if (inArea(newX, newY) && !marked[newX][newY]) {
                    if (dfs(newX, newY, start + 1)) {
                        return true;
                    }
                }
            }
            marked[i][j] = false;
        }
        return false;
    }

    private boolean inArea(int x, int y) {
        return x >= 0 && x < m && y >= 0 && y < n;
    }

    public static void main(String[] args) {

//        char[][] board =
//                {
//                        {'A', 'B', 'C', 'E'},
//                        {'S', 'F', 'C', 'S'},
//                        {'A', 'D', 'E', 'E'}
//                };
//
//        String word = "ABCCED";


        char[][] board = {{'a', 'b'}};
        String word = "ba";
        Solution solution = new Solution();
        boolean exist = solution.exist(board, word);
        System.out.println(exist);
    }
}
```
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int h = board.length, w = board[0].length;
        boolean[][] visited = new boolean[h][w];
        for (int i = 0; i < h; i++) {
            for (int j = 0; j < w; j++) {
                boolean flag = check(board, visited, i, j, word, 0);
                if (flag) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean check(char[][] board, boolean[][] visited, int i, int j, String s, int k) {
        if (board[i][j] != s.charAt(k)) {
            return false;
        } else if (k == s.length() - 1) {
            return true;
        }
        visited[i][j] = true;
        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        boolean result = false;
        for (int[] dir : directions) {
            int newi = i + dir[0], newj = j + dir[1];
            if (newi >= 0 && newi < board.length && newj >= 0 && newj < board[0].length) {
                if (!visited[newi][newj]) {
                    boolean flag = check(board, visited, newi, newj, s, k + 1);
                    if (flag) {
                        result = true;
                        break;
                    }
                }
            }
        }
        visited[i][j] = false;
        return result;
    }
}


```
```
复杂度分析

时间复杂度：一个非常宽松的上界为 O(MN⋅3 
^L)，其中 M, N为网格的长度与宽度，L 为字符串 word 的长度。在每次调用函数 check 时，除了第一次可以进入 44 个分支以外，其余时间我们最多会进入 33 个分支（因为每个位置只能使用一次，所以走过来的分支没法走回去）。由于单词长为 LL，故 \text{check}(i, j, 0)check(i,j,0) 的时间复杂度为 O(3^L)O(3 
L
 )，而我们要执行 O(MN)O(MN) 次检查。然而，由于剪枝的存在，我们在遇到不匹配或已访问的字符时会提前退出，终止递归流程。因此，实际的时间复杂度会远远小于 \Theta(MN \cdot 3^L)Θ(MN⋅3 
L
 )。

空间复杂度：O(MN)。我们额外开辟了 O(MN)O(MN) 的 \textit{visited}visited 数组，同时栈的深度最大为 O(\min(L, MN))O(min(L,MN))。


```



```java
class Solution {
    char [][]board;
    int m;
    int n;
    int [][]dirs={{-1,0},{1,0},{0,-1},{0,1}};
    boolean [][] visited;
    public boolean exist(char[][] board, String word) {
        this.board=board;
        m=board.length;
        n=board[0].length;
        visited=new boolean[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]==word.charAt(0)){
                    if(dfs(i,j,word,0)){
                        return true;
                    }
                }
            }
        }
        return false;
    }
    public boolean dfs(int x,int y,String word,int i){
        if(i==word.length()-1){
            return true;
        }
        visited[x][y]=true;
        for(int []dir:dirs){
            int new_x=x+dir[0];
            int new_y=y+dir[1];
            if(new_x>=0&&new_x<m&&new_y>=0&&new_y<n&&word.charAt(i+1)==board[new_x][new_y]&&!visited[new_x][new_y]){

                if(dfs(new_x,new_y,word,i+1)){
                    return true;
                }
            }
        }
        visited[x][y]=false;
        return false;
    }
}
```