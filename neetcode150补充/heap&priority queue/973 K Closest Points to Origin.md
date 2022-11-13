# 973. K Closest Points to Origin

>Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

>The distance between two points on the X-Y plane is the Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2).

>You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).


注意是k closet 而不是 kth closet
## Solution 1 (heap)
我们可以使用一个大根堆实时维护前 k 个最小的距离平方。

首先我们将前 k 个点的编号（为了方便最后直接得到答案）以及对应的距离平方放入大根堆中，随后从第 k+1 个点开始遍历：如果当前点的距离平方比堆顶的点的距离平方要小，就把堆顶的点弹出，再插入当前的点。当遍历完成后，所有在大根堆中的点就是前 k 个距离最小的点。

```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] array1, int[] array2) {
                return array2[0] - array1[0];
            }
        });
        for (int i = 0; i < k; ++i) {
            pq.offer(new int[]{points[i][0] * points[i][0] + points[i][1] * points[i][1], i});
        }
        int n = points.length;
        for (int i = k; i < n; ++i) {
            int dist = points[i][0] * points[i][0] + points[i][1] * points[i][1];
            if (dist < pq.peek()[0]) {
                pq.poll();
                pq.offer(new int[]{dist, i});
            }
        }
        int[][] ans = new int[k][2];
        for (int i = 0; i < k; ++i) {
            ans[i] = points[pq.poll()[1]];
        }
        return ans;
    }
}
```
时间复杂度：O(nlogk)，其中 n 是数组 points 的长度。由于大根堆维护的是前 k 个距离最小的点，因此弹出和插入操作的单次时间复杂度均为 O(logk)。在最坏情况下，数组里 n 个点都会插入，因此时间复杂度为 O(nlogk)。

空间复杂度：O(k)，因为大根堆中最多有 k 个点。



## sOLUTION 2 (排序)
```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        Arrays.sort(points, new Comparator<int[]>() {
            public int compare(int[] point1, int[] point2) {
                return (point1[0] * point1[0] + point1[1] * point1[1]) - (point2[0] * point2[0] + point2[1] * point2[1]);
            }
        });
        // 积累这个用法
        return Arrays.copyOfRange(points, 0, k);
    }
}
```
时间复杂度：O(nlogn)，其中 n 是数组 points 的长度。算法的时间复杂度即排序的时间复杂度。

空间复杂度：O(logn)，排序所需额外的空间复杂度为 O(logn)。

## solution 3 (快排)
方法三：快速选择（快速排序的思想）

思路和算法

我们也可以借鉴快速排序的思想。

快速排序中的划分操作每次执行完后，都能将数组分成两个部分，其中小于等于分界值 pivot\textit{pivot}pivot 的元素都会被放到左侧部分，而大于 pivot\textit{pivot}pivot 的元素都都会被放到右侧部分。与快速排序不同的是，在本题中我们可以根据 kkk 与 pivot\textit{pivot}pivot 下标的位置关系，只处理划分结果的某一部分（而不是像快速排序一样需要处理两个部分）。

我们定义函数 random_select(left, right, k) 表示划分数组 points\textit{points}points 的 [left,right][\textit{left},\textit{right}][left,right] 区间，并且需要找到其中第 kkk 个距离最小的点。在一次划分操作完成后，设 pivot\textit{pivot}pivot 的下标为 iii，即区间 [left,i−1][\textit{left}, i-1][left,i−1] 中的点的距离都小于等于 pivot\textit{pivot}pivot，而区间 [i+1,right][i+1,\textit{right}][i+1,right] 的点的距离都大于 pivot\textit{pivot}pivot。此时会有三种情况：

如果 k=i−left+1k = i-\textit{left}+1k=i−left+1，那么说明 pivot\textit{pivot}pivot 就是第 kkk 个距离最小的点，我们可以结束整个过程；

如果 k<i−left+1k < i-\textit{left}+1k<i−left+1，那么说明第 kkk 个距离最小的点在 pivot\textit{pivot}pivot 左侧，因此递归调用 random_select(left, i - 1, k)；

如果 k>i−left+1k > i-\textit{left}+1k>i−left+1，那么说明第 kkk 个距离最小的点在 pivot\textit{pivot}pivot 右侧，因此递归调用 random_select(i + 1, right, k - (i - left + 1))。

在整个过程结束之后，第 kkk 个距离最小的点恰好就在数组 points\textit{points}points 中的第 kkk 个位置，并且其左侧的所有点的距离都小于它。此时，我们就找到了前 kkk 个距离最小的点。

作者：力扣官方题解
链接：https://leetcode.cn/problems/k-closest-points-to-origin/solutions/477916/zui-jie-jin-yuan-dian-de-k-ge-dian-by-leetcode-sol/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```java
class Solution {
    Random rand = new Random();

    public int[][] kClosest(int[][] points, int k) {
        int n = points.length;
        random_select(points, 0, n - 1, k);
        return Arrays.copyOfRange(points, 0, k);
    }

    public void random_select(int[][] points, int left, int right, int k) {
        int pivotId = left + rand.nextInt(right - left + 1);
        int pivot = points[pivotId][0] * points[pivotId][0] + points[pivotId][1] * points[pivotId][1];
        swap(points, right, pivotId);
        int i = left - 1;
        for (int j = left; j < right; ++j) {
            int dist = points[j][0] * points[j][0] + points[j][1] * points[j][1];
            if (dist <= pivot) {
                ++i;
                swap(points, i, j);
            }
        }
        ++i;
        swap(points, i, right);
        // [left, i-1] 都小于等于 pivot, [i+1, right] 都大于 pivot
        if (k < i - left + 1) {
            random_select(points, left, i - 1, k);
        } else if (k > i - left + 1) {
            random_select(points, i + 1, right, k - (i - left + 1));
        }
    }

    public void swap(int[][] points, int index1, int index2) {
        int[] temp = points[index1];
        points[index1] = points[index2];
        points[index2] = temp;
    }
}


```

另一版本快排，符合代码规律
```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        quickSort(points, 0, points.length - 1, k);
        return Arrays.copyOfRange(points, 0, k);
    }

    private void quickSort(int[][] points, int left, int right, int k) {
        if (left < right) {
            int pviotPosition = partition(points, left, right);
            if (pviotPosition == k - 1) {
                return;
            } else if (pviotPosition > k - 1) {
                quickSort(points, left, pviotPosition - 1, k);
            } else {
                quickSort(points, pviotPosition + 1, right, k);
            }
        }
    }

    private int partition(int[][] points, int left, int right) {
        swap(points, left, (left + right) / 2);
        int[] pviotNum = points[left];
        int distance = distance(pviotNum);
        while (left < right) {
            while (left < right && distance(points[right]) > distance) {
                right--;
            }
            points[left] = points[right];
            while (left < right && distance(points[left]) <= distance) {
                left++;
            }
            points[right] = points[left];
        }
        points[left] = pviotNum;
        return left;
    }

    private int distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }

    private void swap(int[][] points, int a, int b) {
        int[] temp = points[a];
        points[a] = points[b];
        points[b] = temp;
    }
}
```