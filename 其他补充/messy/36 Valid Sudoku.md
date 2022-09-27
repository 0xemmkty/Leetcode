# 36. Valid Sudoku

>Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
Note:

>A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.

## Solution 1 (Arrays)
显然每个数属于哪个box就只取决于纵坐标，纵坐标为0/1/2的都属于box[0],纵坐标为3/4/5的都属于box[1],纵坐标为6/7/8的都属于box[2].也就是j/3.
而对于9x9的矩阵，我们光根据j/3得到0/1/2还是不够的，可能加上一个3的倍数，例如加0x3,表示本行的box，加1x3，表示在下一行的box，加2x3，表示在下两行的box， 这里的0/1/2怎么来的？和j/3差不多同理，也就是i/3。


```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        boolean[][] row = new boolean[10][10], col = new boolean[10][10], area = new boolean[10][10];        
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                int c = board[i][j];
                if (c == '.') continue;
                int u = c - '0';
                int idx = i / 3 * 3 + j / 3;
                if (row[i][u] || col[j][u] || area[idx][u]) return false;
                row[i][u] = col[j][u] = area[idx][u] = true;
            }
        }
        return true;
    }
}
// very very beautiful method to determind(find) the duplicate value
// three point: 
// 1. int u = c - '0'; convert char to int
// 2. int idx = i / 3 * 3 + j / 3;  divide 9 box
// 3. the way to mark appeared num to true to avoid duplicate (very interesting way!!!i like it)
```
another version
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int [][]row  =new int[9][10];
        int [][]col  =new int[9][10];
        int [][]box  =new int[9][10];
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j]=='.'){
                    continue;
                }
                int curNum = board[i][j]-'0';
                if (row[i][curNum]==1){
                    return false;
                }if (col[j][curNum]==1){
                    return false;
                }
                if (box[j/3 + (i/3) * 3][curNum]==1){
                    return false;
                }
                row[i][curNum]=1;
                col[j][curNum]=1;
                box[j/3 + (i/3) * 3][curNum]=1;
            }
        }
        return true;
    }
}
```
时间复杂度：在固定 9*99∗9 的问题里，计算量不随数据变化而变化。复杂度为 O(1)
空间复杂度：在固定 9*99∗9 的问题里，存储空间不随数据变化而变化。复杂度为 O(1)O(1)

 
 ## Solution 2 (hashmap)
 ```java
 class Solution {
    public boolean isValidSudoku(char[][] board) {
        Map<Integer, Set<Integer>> row  = new HashMap<>(), col = new HashMap<>(), area = new HashMap<>();
        for (int i = 0; i < 9; i++) {
            row.put(i, new HashSet<>());
            col.put(i, new HashSet<>());
            area.put(i, new HashSet<>());
        }
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char c = board[i][j];
                if (c == '.') continue;
                int u = c - '0';
                int idx = i / 3 * 3 + j / 3;
                if (row.get(i).contains(u) || col.get(j).contains(u) || area.get(idx).contains(u)) return false;
                row.get(i).add(u);
                col.get(j).add(u);
                area.get(idx).add(u);
            }
        }
        return true;
    }
}
// compare hashmap with arrays, interesting!! find the similar and different part. hhh
```
时间复杂度：在固定 9∗9 的问题里，计算量不随数据变化而变化。复杂度为 O(1)
空间复杂度：在固定 9*9 的问题里，存储空间不随数据变化而变化。复杂度为 O(1)
虽然时间复杂度一样，但哈希表的更新和查询复杂度为均摊 O(1)O(1)，而定长数组的的更新和查询复杂度则是严格 O(1)O(1)。

因此从执行效率上来说，数组要比哈希表快上不少。


## Solution 3 (bit)  先欠
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int[] row = new int[10], col = new int[10], area = new int[10];
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char c = board[i][j];
                if (c == '.') continue;
                int u = c - '0';
                int idx = i / 3 * 3 + j / 3;
                if ((((row[i] >> u) & 1) == 1) || (((col[j] >> u) & 1) == 1) || (((area[idx] >> u) & 1) == 1)) return false;
                row[i] |= (1 << u);
                col[j] |= (1 << u);
                area[idx] |= (1 << u);
            }
        }
        return true;
    }
}
```
时间复杂度：在固定 9*9 的问题里，计算量不随数据变化而变化。复杂度为 O(1)
空间复杂度：在固定 9*9 的问题里，存储空间不随数据变化而变化。复杂度为 O(1)



