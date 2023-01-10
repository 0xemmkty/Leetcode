## 559. Maximum Depth of N-ary Tree

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

(复习本题前先复习lc104)

## S1 (DFS)
```java
class Solution {
    /*递归法，后序遍历求root节点的高度*/
    public int maxDepth(Node root) {
        if (root == null) return 0;

        int depth = 0;
        if (root.children != null){
            for (Node child : root.children){
                depth = Math.max(depth, maxDepth(child));
            }
        }

        return depth + 1; //中节点
    }  
}
```
时间复杂度：O(n)O(n)
空间复杂度：忽略递归带来的额外空间开销，复杂度为 O(1)O(1)

## S2 (BFS)
```java
class solution {
    /**
     * 迭代法，使用层序遍历
     */
    public int maxDepth(Node root) {
        if (root == null)   return 0;
        int depth = 0;
        Queue<Node> que = new LinkedList<>();
        que.offer(root);
        while (!que.isEmpty())
        {
            depth ++;
            int len = que.size();
            while (len > 0)
            {
                Node node = que.poll();
                for (int i = 0; i < node.children.size(); i++)
                    if (node.children.get(i) != null) 
                        que.offer(node.children.get(i));
                len--;
            }
        }
        return depth;
    }
}
```