# 51. N-Queens
(background: 按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。)

>The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

>Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

>Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

## Solution 1
https://leetcode.cn/problems/n-queens/solution/dai-ma-sui-xiang-lu-51-n-queenshui-su-fa-2k32/
```java
class Solution 
{
    List<List<String>> res;
    public List<List<String>> solveNQueens(int n) 
    {
        res = new ArrayList<>();
        char[][] board = new char[n][n];
        for(char[] chars:board)
        {
            Arrays.fill(chars,'.');
        }
        //创建棋盘board，dfs从 第一行开始遍历
        dfs(board,0,n);
        return res;
    }

    //dfs遍历每一行，其中的for循环遍历每一列
    public void dfs(char[][] board,int r,int n)
    {
        if(r==n)
        {
            res.add(toList(board));
            return;
        }
        for(int i=0;i<n;i++)
        {
            if(isValid(board,r,i,n))
            {
                board[r][i]='Q';
                dfs(board,r+1,n);
                board[r][i]='.';
            }
        }
    }
    //检查某一点 (r,c) 能否放皇后
    public boolean isValid(char[][] board,int r,int c,int n)
    {
        //检查该列
        for(int i=0;i<n;i++)
        {
            if(board[i][c]=='Q')
            {
                return false;
            }
        }
        //检查左上45
        for(int i=r-1,j=c-1;i>=0 && j>=0;i--,j--)
        {
            if(board[i][j]=='Q')
            {
                return false;
            }
        }
        //检查右上45
        for(int i=r-1,j=c+1;i>=0 && j<n;i--,j++)
        {
            if(board[i][j]=='Q')
            {
                return false;
            }
        }
        return true;
    }

    //将 char[][] board 转换为 List<String>
    public List<String> toList(char[][] board)
    {
        List<String> list = new ArrayList<>();
        for(char[] chars:board)
        {
            list.add(String.valueOf(chars));
        }
        return list;
    }
}
```
```java
class Solution {
    List<List<String>> res = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        char[][] chessboard = new char[n][n];
        for (char[] c : chessboard) {
            Arrays.fill(c, '.');
        }
        backTrack(n, 0, chessboard);
        return res;
    }


    public void backTrack(int n, int row, char[][] chessboard) {
        if (row == n) {
            res.add(Array2List(chessboard));
            return;
        }

        for (int col = 0;col < n; ++col) {
            if (isValid (row, col, n, chessboard)) {
                chessboard[row][col] = 'Q';
                backTrack(n, row+1, chessboard);
                chessboard[row][col] = '.';
            }
        }

    }


    public List Array2List(char[][] chessboard) {
        List<String> list = new ArrayList<>();

        for (char[] c : chessboard) {
            list.add(String.copyValueOf(c));
        }
        return list;
    }


    public boolean isValid(int row, int col, int n, char[][] chessboard) {
        // 检查列
        for (int i=0; i<row; ++i) { // 相当于剪枝
            if (chessboard[i][col] == 'Q') {
                return false;
            }
        }

        // 检查45度对角线
        for (int i=row-1, j=col-1; i>=0 && j>=0; i--, j--) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }

        // 检查135度对角线
        for (int i=row-1, j=col+1; i>=0 && j<=n-1; i--, j++) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }
        return true;
    }
}


```