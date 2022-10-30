# 110. Balanced Binary Tree

>Given a binary tree, determine if it is height-balanced.
本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。


## Solution 1 从底至顶（提前阻断）
漂亮的递归！

此方法为本题的最优解法，但“从底至顶”的思路不易第一时间想到。

思路是对二叉树做先序遍历，从底至顶返回子树最大高度，若判定某子树不是平衡树则 “剪枝” ，直接向上返回。

算法流程：

recur(root):

递归返回值：
当节点root 左 / 右子树的高度差<2 ：则返回以节点root为根节点的子树的最大高度，即节点 root 的左右子树中最大高度加 
1 （ max(left, right) + 1 ）；
当节点root 左 / 右子树的高度差 
≥2 ：则返回 −1 ，代表 此子树不是平衡树 。
递归终止条件：当越过叶子节点时，返回高度 0 ；
当左（右）子树高度 left== -1 时，代表此子树的 左（右）子树 不是平衡树，因此直接返回 −1 ；
isBalanced(root) ：

返回值： 若 recur(root) != 1 ，则说明此树平衡，返回 
true ； 否则返回 
false 。
复杂度分析：

时间复杂度 
O(N)： N 为树的节点数；最差情况下，需要递归遍历树的所有节点。
空间复杂度 
O(N)： 最差情况下（树退化为链表时），系统递归需要使用 
O(N) 的栈空间。
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
    public boolean isBalanced(TreeNode root) {
        return recur(root) != -1;
    }

    private int recur(TreeNode root) {
        if (root == null) return 0;
        int left = recur(root.left);
        if(left == -1) return -1;
        int right = recur(root.right);
        if(right == -1) return -1;
        return Math.abs(left - right) < 2 ? Math.max(left, right) + 1 : -1;
    }
}


```

## Solution 2 从顶至底（暴力法）

此方法容易想到，但会产生大量重复计算，时间复杂度较高。

思路是构造一个获取当前节点最大深度的方法 depth(root) ，通过比较此子树的左右子树的最大高度差abs(depth(root.left) - depth(root.right))，来判断此子树是否是二叉平衡树。若树的所有子树都平衡时，此树才平衡。

算法流程：

isBalanced(root) ：判断树 root 是否平衡

特例处理： 若树根节点 root 为空，则直接返回 
true ；
返回值： 所有子树都需要满足平衡树性质，因此以下三者使用与逻辑 
&& 连接；
abs(self.depth(root.left) - self.depth(root.right)) <= 1 ：判断 当前子树 是否是平衡树；
self.isBalanced(root.left) ： 先序遍历递归，判断 当前子树的左子树 是否是平衡树；
self.isBalanced(root.right) ： 先序遍历递归，判断 当前子树的右子树 是否是平衡树；
depth(root) ： 计算树 root 的最大高度

终止条件： 当 root 为空，即越过叶子节点，则返回高度 0 ；
返回值： 返回左 / 右子树的最大高度加 1 。
复杂度分析：

时间复杂度 
O(Nlog2N)： 最差情况下， isBalanced(root) 遍历树所有节点，占用 O(N) ；判断每个节点的最大高度 depth(root) 需要遍历 各子树的所有节点 ，子树的节点数的复杂度为 
O(log 2N) 。
空间复杂度 O(N)： 最差情况下（树退化为链表时），系统递归需要使用 O(N) 的栈空间。

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        return Math.abs(depth(root.left) - depth(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }

    private int depth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(depth(root.left), depth(root.right)) + 1;
    }
}
```