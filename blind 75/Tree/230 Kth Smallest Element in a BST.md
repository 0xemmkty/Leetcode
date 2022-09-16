# 230. Kth Smallest Element in a BST

Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

## Solution 1 ()
>方法一：中序遍历
预备知识

二叉搜索树具有如下性质：

结点的左子树只包含小于当前结点的数。

结点的右子树只包含大于当前结点的数。

所有左子树和右子树自身必须也是二叉搜索树。

二叉树的中序遍历即按照访问左子树——根结点——右子树的方式遍历二叉树；在访问其左子树和右子树时，我们也按照同样的方式遍历；直到遍历完整棵树。

思路和算法

因为二叉搜索树和中序遍历的性质，所以二叉搜索树的中序遍历是按照键增加的顺序进行的。于是，我们可以通过中序遍历找到第 kk 个最小元素。

「二叉树的中序遍历」可以参考「94. 二叉树的中序遍历的官方题解」，具体地，我们使用迭代方法，这样可以在找到答案后停止，不需要遍历整棵树。

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            --k;
            if (k == 0) {
                break;
            }
            root = root.right;
        }
        return root.val;
    }
}
```
>复杂度分析

时间复杂度：O(H+k)，其中 H是树的高度。在开始遍历之前，我们需要 O(H) 到达叶结点。当树是平衡树时，时间复杂度取得最小值O(logN+k)；当树是线性树（树中每个结点都只有一个子结点或没有子结点）时，时间复杂度取得最大值 O(N+k)。

空间复杂度：O(H)，栈中最多需要存储 H 个元素。当树是平衡树时，空间复杂度取得最小值 O(logN)；当树是线性树时，空间复杂度取得最大值 O(N)。

## Solution 2 (记录子树的结点数)
```java
// 返回二叉搜索树中第k小的元素
    public int kthSmallest(int k) {
        TreeNode node = root;
        while (node != null) {
            int left = getNodeNum(node.left);
            if (left < k - 1) {
                node = node.right;
                k -= left + 1;
            } else if (left == k - 1) {
                break;
            } else {
                node = node.left;
            }
        }
        return node.val;
    }

    // 统计以node为根结点的子树的结点数
    private int countNodeNum(TreeNode node) {
        if (node == null) {
            return 0;
        }
        nodeNum.put(node, 1 + countNodeNum(node.left) + countNodeNum(node.right));
        return nodeNum.get(node);
    }

    // 获取以node为根结点的子树的结点数
    private int getNodeNum(TreeNode node) {
        return nodeNum.getOrDefault(node, 0);
    }
}
复杂度分析

时间复杂度：预处理的时间复杂度为 O(N)，其中 NN 是树中结点的总数；我们需要遍历树中所有结点来统计以每个结点为根结点的子树的结点数。搜索的时间复杂度为O(H)，其中 HH 是树的高度；当树是平衡树时，时间复杂度取得最小值 O(logN)；当树是线性树时，时间复杂度取得最大值 O(N)。

空间复杂度：O(N)，用于存储以每个结点为根结点的子树的结点数。

```