# 572. Subtree of Another Tree

>Given the roots of two binary trees **root** and **subRoot**, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.
A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

## Solution 1 ()
>**解题思路**
要判断一个树 t 是不是树 s 的子树，那么可以判断 t 是否和树 s 的任意子树相等。那么就转化成 100. Same Tree。
即，这个题的做法就是在 s 的每个子节点上，判断该子节点是否和 t 相等。
判断两个树是否相等的三个条件是与的关系，即：
当前两个树的根节点值相等；
并且，s 的左子树和 t 的左子树相等；
并且，s 的右子树和 t 的右子树相等。
而判断 t 是否为 s 的子树的三个条件是或的关系，即：
当前两棵树相等；
或者，t 是 s 的左子树；
或者，t 是 s 的右子树。
判断 是否是相等的树 与 是否是子树 的代码简直是对称美啊~


```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if(s == null && t == null) return true;
        if(s == null || t == null) return false;
        return isSametree(s,t) || isSubtree(s.left,t) || isSubtree(s.right,t);
    }
    public boolean isSametree(TreeNode s, TreeNode t) {
        if(s == null && t == null) return true;
        if(s == null || t == null) return false;
        return s.val == t.val && isSametree(s.left,t.left) && isSametree(s.right,t.right);
    }
}
```


```
复杂度分析

时间复杂度：对于每一个 s 上的点，都需要做一次深度优先搜索来和 t匹配，匹配一次的时间代价是 O(|t|)，那么总的时间代价就是 O(∣s∣×∣t∣)。故渐进时间复杂度为 O(∣s∣×∣t∣)。
空间复杂度：假设 s 深度为 d_s,t 的深度为 d_t
​
 ，任意时刻栈空间的最大使用代价是O(max{d_s,d_t})。故渐进空间复杂度为O(max{d_s,d_t}).


```
同思路，另一写法
```java
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        return dfs(s, t);
    }

    public boolean dfs(TreeNode s, TreeNode t) {
        if (s == null) {
            return false;
        }
        return check(s, t) || dfs(s.left, t) || dfs(s.right, t);
    }

    public boolean check(TreeNode s, TreeNode t) {
        if (s == null && t == null) {
            return true;
        }
        if (s == null || t == null || s.val != t.val) {
            return false;
        }
        return check(s.left, t.left) && check(s.right, t.right);
    }
}


```