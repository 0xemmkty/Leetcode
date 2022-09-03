48. Rotate Image

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

（即第一排的左边变成第一排的右边）
推荐 solu 3
## Solution 1 （找规律,建立辅助数组）
(题中有说不能分配额外数组..？)
>**找规律,find the rule**很重要！！
对于矩阵中第 i 行的第 j 个元素，在旋转后，它出现在倒数第 i 列的第 j 个位置。

>我们将其翻译成代码。由于矩阵中的行列从 0 开始计数，因此对于矩阵中的元素matrix[row][col]，在旋转后，它的新位置为 matrix(new)[col][n−row−1]。


```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        int[][] matrix_new = new int[n][n];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                matrix_new[j][n - i - 1] = matrix[i][j];
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                matrix[i][j] = matrix_new[i][j];
            }
        }
    }
}

```
时间复杂度：O(N^2)，其中 N 是 matrix 的边长。

空间复杂度：O(N^2)
 。我们需要使用一个和 matrix 大小相同的辅助数组。


## Solution 2 （原地旋转，Rotate in place）
使用temp来储存值，这样就不怕被覆盖了
一次换四个  （列出来很好理解的
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < (n + 1) / 2; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1] = temp;
            }
        }
    }
}
// 将矩阵分诚四块
// 奇偶情况
//奇数 中间的值不变
// 注意两个边界 i < n / 2，j < (n + 1)
```
时间复杂度：O(N^2)，其中 N 是 matrix 的边长。我们需要枚举的子矩阵大小为 O(⌊n/2⌋×⌊(n+1)/2⌋)=O(N^2)。

空间复杂度：O(1)。为原地旋转。

## Solution 3 (翻转代替旋转)
也是找出的规律，左右翻一次，对角线翻一次
使用temp解决覆盖问题
（翻转是不会重合的两部分，很优秀！比旋转优秀！）
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 水平翻转
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < n; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = temp;
            }
        }
        // 主对角线翻转
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
}

```
