# 218. The Skyline Problem (欠)
https://leetcode.com/problems/the-skyline-problem/description/
A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return the skyline formed by these buildings collectively.

The geometric information of each building is given in the array buildings where buildings[i] = [lefti, righti, heighti]:

lefti is the x coordinate of the left edge of the ith building.
righti is the x coordinate of the right edge of the ith building.
heighti is the height of the ith building.
You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

The skyline should be represented as a list of "key points" sorted by their x-coordinate in the form [[x1,y1],[x2,y2],...]. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate 0 and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.

Note: There must be no consecutive horizontal lines of equal height in the output skyline. For instance, [...,[2 3],[4 5],[7 5],[11 5],[12 7],...] is not acceptable; the three lines of height 5 should be merged into one in the final output as such: [...,[2 3],[4 5],[12 7],...]


优先队列 + 哈希表：哈希表的作用是延迟删除，需要删除的时候现在哈希表里做一个记录，等到它上浮到堆顶的时候才删除；
二分搜索树：得使用映射，Java 中是 TreeMap，相当于上面「优先队列 + 哈希表」的组合（这里感

 ## Solution 1(优先队列)
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.PriorityQueue;

public class Solution {

    public List<List<Integer>> getSkyline(int[][] buildings) {
        // 第 1 步：预处理
        List<int[]> buildingPoints = new ArrayList<>();
        for (int[] b : buildings) {
            // 负号表示左边高度的转折点
            buildingPoints.add(new int[]{b[0], -b[2]});
            buildingPoints.add(new int[]{b[1], b[2]});
        }

        // 第 2 步：按照横坐标排序，横坐标相同的时候，高度高的在前面
        buildingPoints.sort((a, b) -> {
            if (a[0] != b[0]) {
                return a[0] - b[0];
            } else {
                // 注意：这里因为左端点传进去的时候，数值是负的，在比较的时候还按照升序排序
                return a[1] - b[1];
            }
        });

        // 第 3 步：扫描一遍动态计算出结果
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        // 哈希表，记录「延迟删除」的元素，key 为元素，value 为需要删除的次数
        Map<Integer, Integer> delayed = new HashMap<>();

        // 最开始的时候，需要产生高度差，所以需要加上一个高度为 0，宽度为 0 的矩形
        maxHeap.offer(0);
        // 为了计算高度差，需要保存之前最高的高度
        int lastHeight = 0;
        List<List<Integer>> res = new ArrayList<>();
        for (int[] buildingPoint : buildingPoints) {
            if (buildingPoint[1] < 0) {
                // 说明此时是「从下到上」，纵坐标参与选拔最大值，请见「规则 1」
                maxHeap.offer(-buildingPoint[1]);
            } else {
                // 不是真的删除 buildingPoint[1]，把它放进 delayed，等到堆顶元素是 buildingPoint[1] 的时候，才真的删除
                delayed.put(buildingPoint[1], delayed.getOrDefault(buildingPoint[1], 0) + 1);
            }

            // 如果堆顶元素在延迟删除集合中，才真正删除，这一步可能执行多次，所以放在 while 中
            // while (true) 都是可以的，因为 maxHeap 一定不会为空
            while (!maxHeap.isEmpty()) {
                int curHeight = maxHeap.peek();
                if (delayed.containsKey(curHeight)) {
                    delayed.put(curHeight, delayed.get(curHeight) - 1);
                    if (delayed.get(curHeight) == 0) {
                        delayed.remove(curHeight);
                    }
                    maxHeap.poll();
                } else {
                    break;
                }
            }

            int curHeight = maxHeap.peek();
            // 有高度差，才有关键点出现
            if (curHeight != lastHeight) {
                // 正在扫过的左端点的值
                res.add(Arrays.asList(buildingPoint[0], curHeight));
                // 当前高度成为计算高度差的标准
                lastHeight = curHeight;
            }
        }
        return res;
    }
}

```
时间复杂度：O(N \log N)O(NlogN)


```java
class Solution {
    public List<List<Integer>> getSkyline(int[][] bs) {
        List<List<Integer>> ans = new ArrayList<>();
        List<int[]> ps = new ArrayList<>();
        for (int[] b : bs) {
            int l = b[0], r = b[1], h = b[2];
            ps.add(new int[]{l, h, -1});
            ps.add(new int[]{r, h, 1});
        }
        /**
         * 先严格按照横坐标进行「从小到大」排序
         * 对于某个横坐标而言，可能会同时出现多个点，应当按照如下规则进行处理：
         * 1. 优先处理左端点，再处理右端点
         * 2. 如果同样都是左端点，则按照高度「从大到小」进行处理（将高度增加到优先队列中）
         * 3. 如果同样都是右端点，则按照高度「从小到大」进行处理（将高度从优先队列中删掉）
         */
        Collections.sort(ps, (a, b)->{
            if (a[0] != b[0]) return a[0] - b[0];
            if (a[2] != b[2]) return a[2] - b[2];
            if (a[2] == -1) {
                return b[1] - a[1];
            } else {
                return a[1] - b[1];
            }
        });
        // 记录进行了删除操作的高度，以及删除次数
        Map<Integer, Integer> map = new HashMap<>();
        PriorityQueue<Integer> q = new PriorityQueue<>((a,b)->b-a);
        int prev = 0;
        q.add(prev);
        for (int[] p : ps) {
            int point = p[0], height = p[1], flag = p[2];
            if (flag == -1) {
                q.add(height);
            } else {
                map.put(height, map.getOrDefault(height, 0) + 1);
            }

            while (!q.isEmpty()) {
                int peek = q.peek();
                if (map.containsKey(peek)) {
                    if (map.get(peek) == 1) map.remove(peek);
                    else map.put(peek, map.get(peek) - 1);
                    q.poll();
                } else {
                    break;
                }
            }

            int cur = q.peek();
            if (cur != prev) {
                List<Integer> list = new ArrayList<>();
                list.add(point);
                list.add(cur);
                ans.add(list);
                prev = cur;
            }
        }
        return ans;
    }
}

作者：AC_OIer
链接：https://leetcode.cn/problems/the-skyline-problem/solution/gong-shui-san-xie-sao-miao-xian-suan-fa-0z6xc/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```