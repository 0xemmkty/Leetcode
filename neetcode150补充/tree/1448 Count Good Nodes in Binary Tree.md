# 1448. Count Good Nodes in Binary Tree

>Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

>**Return the number of good nodes** in the binary tree.

(max value in the path starting from the node)

## Soltion 1 (dfs)
简单dfs思路, 保存从根节点到当前节点的路径中的最大值, 与当前节点的值进行比较即可.
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
    public int goodNodes(TreeNode root) {
        if (root == null) return 0;
        dfs(root, root.val);
        return ans;
    }

    int ans = 0;
    void dfs(TreeNode node, int curMax) {
        if (node == null) return;
        if (node.val >= curMax) {
            ans++;
            curMax = node.val;
        }
        dfs(node.left, curMax);
        dfs(node.right, curMax);
    }
}
```

