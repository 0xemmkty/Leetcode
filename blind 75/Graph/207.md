# 207. Course Schedule
>There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = **[ai, bi] **indicates that you must take **course bi first if you want to take course ai**.
For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.

## Solution 1 （BFS+拓扑排序）
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indegrees = new int[numCourses];
        List<List<Integer>> adjacency = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < numCourses; i++)
            adjacency.add(new ArrayList<>());
        // Get the indegree and adjacency of every course.
        for(int[] cp : prerequisites) {
            indegrees[cp[0]]++;
            adjacency.get(cp[1]).add(cp[0]);
        }
        // Get all the courses with the indegree of 0.
        for(int i = 0; i < numCourses; i++)
            if(indegrees[i] == 0) queue.add(i);
        // BFS TopSort.
        while(!queue.isEmpty()) {
            int pre = queue.poll();
            numCourses--;
            for(int cur : adjacency.get(pre))
                if(--indegrees[cur] == 0) queue.add(cur);
        }
        return numCourses == 0;
    }
}
```


## Solution 2(DFS+贪心)
时间复杂度O(N+M)： 遍历一个图需要访问所有节点和所有临边，N和 M分别为节点数量和临边数量；
 
 空间复杂度 O(N+M)： 为建立邻接表所需额外空间，adjacency 长度为 N ，并存储 M 条临边的数据。

### 更新！！最快版本 3ms （自写）
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graphic = new List[numCourses];
        // flags is a array, length is the number of course
        int[] flags = new int[numCourses];
    //for every "first node" in graph, create a array (totlly i)
    for (int i = 0; i < numCourses; i++) {
        graphic[i] = new ArrayList<>();
    }
    //add every prerequisites course in the right place
    //pre[0] represent the course, pre[1] represent the all prerequisite course before take course.
    for (int[] pre : prerequisites) {
        graphic[pre[0]].add(pre[1]);
    }
    // dfs. if have circle, return false.
        for(int i = 0; i < numCourses; i++)
            if(!dfs(graphic, flags, i)) return false;
        return true;
    }

    private boolean dfs(List<Integer>[] graphic, int[] flags, int i) {
        //is current term has visited(value is 1)? yes: there is a circle.
        if(flags[i] == 1) return false;
        //previous term has visited(value is -1)?yes: previous term has vetified that there is no circle, therefore in this term we can direcly return true(there is no cicle).
        if(flags[i] == -1) return true;
        //above two has skip(first time visit, value is 0), set visited(value set to 1)
        flags[i] = 1;
        //search one course's all prerequisites course, for each prerequisites course search all prerequisites course(the essent od DFS)
        for(int j : graphic[i])
            if(!dfs(graphic, flags, j)) return false;
        //every time finish search, remenber to set flags to be -1 means this term is end and I have visited this course
        flags[i] = -1;
        return true;
    }
}
// we use graphic data structure to store.
//写法;List<Integer>[] graphic = new List[numCourses];
//相当于是一个纵向的list和横向的array
//define flag. flags is a array, length is the number of course.
// it has value 0(not visited),1(current term visited),-1(has visited and there is no circle, so if meet can return directly)
//notice the determine statements
//in dfs(). first find if has visited in same term?
// next find if has visited in previous term?
//set 1 for the first time(this time) visit
//dfs  true false
//set end of this term and has visited (-1)
```


>题目描述：一个课程可能会先修课程，判断给定的先修课程规定是否合法。
本题不需要使用拓扑排序，只需要检测有向图是否存在环即可。(看下一个解析吧)
```java
class Solution {
   public boolean canFinish(int numCourses, int[][] prerequisites) {
    List<Integer>[] graphic = new List[numCourses];
    for (int i = 0; i < numCourses; i++) {
        graphic[i] = new ArrayList<>();
    }
    for (int[] pre : prerequisites) {
        graphic[pre[0]].add(pre[1]);
    }
    boolean[] globalMarked = new boolean[numCourses];
    boolean[] localMarked = new boolean[numCourses];
    for (int i = 0; i < numCourses; i++) {
        if (hasCycle(globalMarked, localMarked, graphic, i)) {
            return false;
        }
    }
    return true;
}

private boolean hasCycle(boolean[] globalMarked, boolean[] localMarked,
                         List<Integer>[] graphic, int curNode) {

    if (localMarked[curNode]) {
        return true;
    }
    if (globalMarked[curNode]) {
        return false;
    }
    globalMarked[curNode] = true;
    localMarked[curNode] = true;
    for (int nextNode : graphic[curNode]) {
        if (hasCycle(globalMarked, localMarked, graphic, nextNode)) {
            return true;
        }
    }
    localMarked[curNode] = false;
    return false;
}
}
```
（思路一样，另一写法）
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adjacency = new ArrayList<>();
        for(int i = 0; i < numCourses; i++)
            adjacency.add(new ArrayList<>());
        int[] flags = new int[numCourses];
        for(int[] cp : prerequisites)
            adjacency.get(cp[1]).add(cp[0]);
        for(int i = 0; i < numCourses; i++)
            if(!dfs(adjacency, flags, i)) return false;
        return true;
    }
    private boolean dfs(List<List<Integer>> adjacency, int[] flags, int i) {
        if(flags[i] == 1) return false;
        if(flags[i] == -1) return true;
        flags[i] = 1;
        for(Integer j : adjacency.get(i))
            if(!dfs(adjacency, flags, j)) return false;
        flags[i] = -1;
        return true;
    }
}
```

```
记住这三个标志位对应的状态
i == 0 ： 干净的，未被 DFS 访问
i == -1：其他节点启动的 DFS 访问过了，路径没问题，不需要再访问了
i == 1  ：本节点启动的 DFS 访问过了，一旦遇到了也说明有环了

有0、1、2、3、4、5  共6门课程。 [[5,3],[5,4],[3,0],[3,1],[4,1],[4,2]]。学5之前要先学3，学5之前要先学4......

0          邻接表
   \       下标  0     1     2     3    4    5   同时也表示要先学的课程
   /   3        [3]  [3,4]  [4]   [5]  [5]  [ ]  学完上面的课程才能学的课程，例如学完0才能学3，学完1才能学3和4
1        \
   \     /  5     这个邻接表的创建是通过        for(int[] cp : prerequisites)
      4                                           adjacency.get(cp[1]).add(cp[0]);         实现的
   /  
2

然后就是遍历+标记判断有无环：

1、从课程0启动DFS，先判断下，哦，节点0还没被访问，将flag[0]置1，表明被当前节点启动的dfs访问过了，在访问0时
就通过以下代码一连串把3、5都访问了,返回true之前标志位置-1，这样从其他节点进来看到标志位-1时就无需再访问了，
直接返回true.
if(flags[i] == 1) return false;  //先判断再修改标志位
if(flags[i] == -1) return true;  //别的dfs路径访问过了，我不需要访问了
flags[i] = 1;   //只有这个标志位是干净的，别人还没有动过，我才能标记为1，说明本次dfs我遍历过它
for(Integer j : adjacency.get(i))
   if(!dfs(adjacency, flags, j))
       return false;
flags[i] = -1;    //只有一次DFS完整结束了，才能执行到这一步，标记为-1，说明这条路没问题，再遇到不需要遍历了
return true;
2.从课程1启动DFS一样的道理，3、5上一步已经访问过了标志位为-1，这一步就不访问了，只要访问4，并把标志位置为-1.,
3、剩下的同理。这里是举了个无环的情况，那有环是什么样子呢？
下标  0     1     2     3    4    5   先学的课程
     [3]  [3,4]  [4]   [5]  [5]  [3 ]  这里多加了一个，先学5才能学3，这回肯定就有问题了

在第一步从课程0启动DFS，一连串访问3，5，从5又遍历到3时，由于我们本节点启动的dfs已经访问过3了，标志位为1，
所以碰到标志位为1，说明有环了，直接返回false;一旦某次df返回了false,整个遍历就结束了，返回最终结果false
 ```