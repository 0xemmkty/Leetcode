## 74. Search a 2D Matrix
乱入：

>Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:

>Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.

## Solution 1(坐标法)
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int rows = matrix.length - 1, columns = 0;
        while (rows >= 0 && columns < matrix[0].length) {
            int num = matrix[rows][columns];
            if (num == target) {
                return true;
            } else if (num > target) {
                rows--;
            } else {
                columns++;
            }
        }
        return false;
    }
}
```

## Solution 2(二分法)
将二维矩阵当一维矩阵
```java
class Solution {
    public boolean searchMatrix(int[][] mat, int t) {
        int m = mat.length, n = mat[0].length;
        int l = 0, r = m * n - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (mat[mid / n][mid % n] <= t) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return mat[r / n][r % n] == t;
    }
}
```
时间复杂度：O(log(m∗n))
空间复杂度：O(1)
```
同方法不同解法
解题思路：

将这个二维数组想象成一个一维的，比如是一个4*4的数组，那么可以转化为一个长度为16的一维数组，之后我们就可以通过传统的二分手段：取left为0，right为m * n - 1，通过条件left <= right来判断整个查找是否结束。

因为转为了一维，那么对于中位数mid，我们需要将其转化为在二维中的下标，整个的计算方式为：


行下标 = mid / n
列下标 = mid % n
除了有一个转化的过程，其他的地方和传统二分一模一样，非常简单。
```
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        int left = 0, right = m * n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int i = mid / n;
            int j = mid % n;
            if (matrix[i][j] == target) {
                return true;
            }
            if (matrix[i][j] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
}

```


## solution 3（抽象成一右上角为根的bst二叉树）
```java
class Solution {
    int m, n;
    public boolean searchMatrix(int[][] mat, int t) {
        m = mat.length; n = mat[0].length;
        int x = 0, y = n - 1;
        while (check(x, y) && mat[x][y] != t) {
            if (mat[x][y] > t) {
                y--;
            } else {
                x++;
            }
        }
        return check(x, y) && mat[x][y] == t;
    }
    boolean check(int x, int y) {
        return x >= 0 && x < m && y >= 0 && y < n;
    }
}
```
时间复杂度：O(m+n)
空间复杂度：O(1)