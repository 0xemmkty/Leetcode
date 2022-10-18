# 332. Reconstruct Itinerary

>You are given a list of airline tickets where tickets[i] = [fromi, toi]represent the **departure and the arrival airports** of one flight. Reconstruct the itinerary in order and return it.

>All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

>For example, the itinerary ["JFK", "LGA"] has a smaller **lexical order** than ["JFK", "LGB"].
You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

```
EXAMPLE
(pic is imcomplete)
SFO

↑

JFK         ATL



Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.
```

优化 from 1 to 3，见优化

## Solution 1
>https://leetcode.cn/problems/reconstruct-itinerary/solution/javadfsjie-fa-by-pwrliang/  (有一张非常直接的图可以看)

>重点：**对“孤岛节点”的处理**

>图 2 这个例子可以看出，我们别无选择必须先从 JFK 到 NRT 再回 JFK，最后到达 KUL 作为终点。如果我们按照字典顺序先到 KUL，就进入了 “死路”。但是上一个例子我们提到了，优先访问字典顺序小的顶点，那么我们第一次肯定是先到 KUL，这就走不通了，那怎么解决呢？当我们采用 DFS 方式遍历图时，需要将访问到的节点逆序插入到结果集。因此第一个访问到的节点将出现在结果集最后面，而我们是以顺序的方式来查看结果。如果第一个访问的节点是 “孤岛节点”，他会出现在结果集的最后。当我们顺序读取结果集时，这种 “孤岛节点” 是最后遇到的，是图遍历的终点，这样就没有问题了。
```java
import java.util.*;
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        // 因为逆序插入，所以用链表
        List<String> ans = new LinkedList<>();
        if (tickets == null || tickets.size() == 0)
            return ans;
        Map<String, List<String>> graph = new HashMap<>();
        for (List<String> pair : tickets) {
            // 因为涉及删除操作，我们用链表
            List<String> nbr = graph.computeIfAbsent(pair.get(0), k -> new LinkedList<>());
            nbr.add(pair.get(1));
        }
        // 按目的顶点排序
        graph.values().forEach(x -> x.sort(String::compareTo));
        visit(graph, "JFK", ans);
        return ans;
    }
    // DFS方式遍历图，当走到不能走为止，再将节点加入到答案
    private void visit(Map<String, List<String>> graph, String src, List<String> ans) {
        List<String> nbr = graph.get(src);
        while (nbr != null && nbr.size() > 0) {
            String dest = nbr.remove(0);
            visit(graph, dest, ans);
        }
        ans.add(0, src); // 逆序插入
    }
}
```
```
因为每个顶点都要访问一次，每条边都要访问一次，时间复杂度应为 O(∣V∣+∣E∣)，还要记得对临接点排序的时间复杂度 O(∣E∣log∣E∣)，算法整体时间复杂度为O(∣E∣log∣E∣)；如果整个图是链式的，那么调用栈最深，空间复杂度应为 O(∣E∣)。


```
## Solution 2
其实我们可以不对临接点排序，而是使用小顶堆（Java可以用优先队列）。这样我们删除边的操作和访问最小字典顺序顶点可以用出队操作代替，时间复杂度应该会比排序再删除要低一些。
```java
import java.util.*;
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        // 因为逆序插入，所以用链表
        List<String> ans = new LinkedList<>();
        if (tickets == null || tickets.size() == 0)
            return ans;
        Map<String, PriorityQueue<String>> graph = new HashMap<>();
        for (List<String> pair : tickets) {
            // 因为涉及删除操作，我们用链表
            PriorityQueue<String> nbr = graph.computeIfAbsent(pair.get(0), k -> new PriorityQueue<>());
            nbr.add(pair.get(1));
        }
        visit(graph, "JFK", ans);
        return ans;
    }
    // DFS方式遍历图，当走到不能走为止，再将节点加入到答案
    private void visit(Map<String, PriorityQueue<String>> graph, String src, List<String> ans) {
        PriorityQueue<String> nbr = graph.get(src);
        while (nbr != null && nbr.size() > 0) {
            String dest = nbr.poll();
            visit(graph, dest, ans);
        }
        ans.add(0, src); // 逆序插入
    }
}
```

## Solution 3
进一步优化，将递归算法改为迭代算法：

```java
import java.util.*;

class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        // 因为逆序插入，所以用链表
        List<String> ans = new LinkedList<>();

        if (tickets == null || tickets.size() == 0)
            return ans;

        Map<String, PriorityQueue<String>> graph = new HashMap<>();

        for (List<String> pair : tickets) {
            // 因为涉及删除操作，我们用链表
            PriorityQueue<String> nbr = graph.computeIfAbsent(pair.get(0), k -> new PriorityQueue<>());
            nbr.add(pair.get(1));
        }

        // 按目的顶点排序

        visit(graph, "JFK", ans);

        return ans;
    }

    // DFS方式遍历图，当走到不能走为止，再将节点加入到答案
    private void visit(Map<String, PriorityQueue<String>> graph, String src, List<String> ans) {

        Stack<String> stack = new Stack<>();

        stack.push(src);

        while (!stack.isEmpty()) {
            PriorityQueue<String> nbr;

            while ((nbr = graph.get(stack.peek())) != null &&
                    nbr.size() > 0) {
                stack.push(nbr.poll());
            }
            ans.add(0, stack.pop());
        }
    }
}
```