# 52. N-Queens II

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.

## Solution 1 (DFS + 位运算剪枝) 有点难，见法2
解题思路：
这个解法非常经典，所以我觉得应该放到题解里面，核心思路是这样：

使用常规深度优先一层层搜索
使用三个整形分别标记每一层哪些格子可以放置皇后，这三个整形分别代表列、左斜下、右斜下（_col, ld, rd_），二进制位为 11 代表不能放置，00 代表可以放置
核心两个位运算：
x & -x 代表除最后一位 11 保留，其它位全部为 00
x & (x - 1) 代表将最后一位 11 变成 00

作者：makeex
链接：https://leetcode.cn/problems/n-queens-ii/solution/dfs-wei-yun-suan-jian-zhi-by-makeex/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```java
class Solution {
    private int ans = 0;

    public int totalNQueens(int n) {
        dfs(n, 0, 0, 0, 0);
        return this.ans;
    }

    private void dfs(int n, int row, int cols, int pie, int na) {
        if (row == n) {
            this.ans ++;
            return;
        }
        int positions = ((1 << n) - 1) & (~(cols | pie | na));
        while (positions != 0) {
            int p = positions & (-positions);
            positions &= (positions - 1);
            dfs(n, row + 1, cols | p, (pie | p) << 1, (na | p) >> 1);
        }
    }
}
```
```java
class Solution {
    int res=0;
    public int totalNQueens(int n) {
        dfs(n,0,0,0,0);//位中0表示能放皇后
        return res;
    }

    //n表示多少个皇后，row表示放第几行的皇后，col中位为1表示不能放皇后，ld和rd同理
    public void dfs(int n,int row,int col,int ld,int rd){
        if(row==n){
            res++;
            return;
        }
        int canQueens=~(col|ld|rd)&((1<<n)-1);
        //col|ld|rd找出不可以放置皇后的位置（1表示），取反之后表示1代表能放皇后的位置
        //(1<<n)-1先把1变成1后面n个0，减1之后就是低位存在n个连续的1，即能放皇后

        //没有放完全部皇后的位置
        while(canQueens>0){
            int place=canQueens&(-canQueens);//取最低位的1，表示选中皇后的位置，比如010110变成了010010
            dfs(n,row+1,col|place,(ld|place)<<1,(rd|place)>>1);
            //ROW+1表示下一行，col|place表示目前已经放置了皇后的列（1表示），后面的皇后不能放这些列1
            //ld表示到第row行时由于对角线（↙）产生的不能放皇后的位置，
            //(ld|place)<<1表示当前已放置皇后到第row+1行所产生的不能放皇后的位置
            //rd即↗同理

            canQueens&=(canQueens-1);//当前行的皇后少了一个选择（canQueens中最后那个1）
        }

    }
}
```

## Solution 2
```java
class Solution {
    char[][] board;
    int n;
    int res = 0;

    public int totalNQueens(int _n) {
        /*
        与51.N皇后十分类似:这题相比更简单
         */
        n = _n;
        board = new char[n][n];
        for (int i = 0; i < n; i++) {
            // '.'表示没有放置国王
            Arrays.fill(board[i], '.');
        }
        // 按照行的顺序进行遍历
        dfs(0);
        return res;
    }

    private void dfs(int r) {
        // 遍历到n,说明[0,n-1]行已经全部遍历完毕,合法结果+1
        if (r == n) res++;
        // 遍历第r行的所有列
        for (int c = 0; c < n; c++) {
            // 无效的直接跳过
            if (!valid(r, c)) continue;
            // 可以放置的在该处放置国王
            board[r][c] = 'Q';
            // 继续判断下一行(因为一行只能放一个)
            dfs(r + 1);
            // 撤回(回到原来这行肯定要删除掉刚刚标记的'Q')
            board[r][c] = '.';
        }
    }

    // 判定在[r,c]坐标放置国王是否合法(排除法)
    private boolean valid(int r, int c) {
        // 同一行必定不会重复放置(递归逻辑决定)
        // 因此只要判断列与两条斜边即可
        // 列[0,r-1]
        for (int i = 0; i < r; i++) {
            if (board[i][c] != '.') return false;
        }
        // 斜边:左上->右下
        for (int i = r - 1, j = c - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] != '.') return false;
        }
        // 斜边:右上->左下
        for (int i = r - 1, j = c + 1; i >= 0 && j <= n - 1; i--, j++) {
            if (board[i][j] != '.') return false;
        }
        // 排除后的情形就是合法的
        return true;
    }
}
```