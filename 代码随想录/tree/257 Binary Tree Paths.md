## 257. Binary Tree Paths

Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

这个网址覆盖了多种解法，写的不错 https://leetcode.cn/problems/binary-tree-paths/solution/257-er-cha-shu-de-suo-you-lu-jing-tu-wen-jie-xi-by/
## S1(DFS)
二叉树DFS算法如下
```java
    public static void treeDFS(TreeNode root) {
        //当前节点为空直接返回
        if (root == null)
            return;
        //打印当前节点的值
        System.out.println(root.val);
        //然后递归遍历左右子节点
        treeDFS(root.left);
        treeDFS(root.right);
    }

```
仿造上面的来写本题
```java
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        dfs(root, "", res);
        return res;
    }

    private void dfs(TreeNode root, String path, List<String> res) {
        //如果为空，直接返回
        if (root == null)
            return;
        //如果是叶子节点，说明找到了一条路径，把它加入到res中
        if (root.left == null && root.right == null) {
            res.add(path + root.val);
            return;
        }
        //如果不是叶子节点，在分别遍历他的左右子节点
        dfs(root.left, path + root.val + "->", res);
        dfs(root.right, path + root.val + "->", res);
    }

```
