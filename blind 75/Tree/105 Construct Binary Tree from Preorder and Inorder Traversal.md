# 105. Construct Binary Tree from Preorder and Inorder Traversal

>Given two integer arrays **preorder and inorder** where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

## Solution 1 (recrusion)
>先序遍历的顺序是根节点，左子树，右子树。中序遍历的顺序是左子树，根节点，右子树。

所以我们只需要根据先序遍历得到根节点，然后在中序遍历中找到根节点的位置，它的左边就是左子树的节点，右边就是右子树的节点。

生成左子树和右子树就可以递归的进行了。

比如上图的例子，我们来分析一下。
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
首先根据 preorder 找到根节点是 3
    
然后根据根节点将 inorder 分成左子树和右子树
左子树
inorder [9]

右子树
inorder [15,20,7]

把相应的前序遍历的数组也加进来
左子树
preorder[9] 
inorder [9]

右子树
preorder[20 15 7] 
inorder [15,20,7]

现在我们只需要构造左子树和右子树即可，成功把大问题化成了小问题
然后重复上边的步骤继续划分，直到 preorder 和 inorder 都为空，返回 null 即可

```
>事实上，我们不需要真的把 preorder 和 inorder 切分了，只需要用分别用两个指针指向开头和结束位置即可。注意下边的两个指针指向的数组范围是包括左边界，不包括右边界。
对于下边的树的合成。

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
    return buildTreeHelper(preorder, 0, preorder.length, inorder, 0, inorder.length);
}

private TreeNode buildTreeHelper(int[] preorder, int p_start, int p_end, int[] inorder, int i_start, int i_end) {
    // preorder 为空，直接返回 null
    if (p_start == p_end) {
        return null;
    }
    int root_val = preorder[p_start];
    TreeNode root = new TreeNode(root_val);
    //在中序遍历中找到根节点的位置
    int i_root_index = 0;
    for (int i = i_start; i < i_end; i++) {
        if (root_val == inorder[i]) {
            i_root_index = i;
            break;
        }
    }
    int leftNum = i_root_index - i_start;
    //递归的构造左子树
    root.left = buildTreeHelper(preorder, p_start + 1, p_start + leftNum + 1, inorder, i_start, i_root_index);
    //递归的构造右子树
    root.right = buildTreeHelper(preorder, p_start + leftNum + 1, p_end, inorder, i_root_index + 1, i_end);
    return root;
}


}
//思路简单，即看即会
```

>**优化**
上边的代码很好理解，但存在一个问题，在中序遍历中找到根节点的位置每次都得遍历中序遍历的数组去寻找，参考这里 ，我们可以用一个HashMap把中序遍历数组的每个元素的值和下标存起来，这样寻找根节点的位置就可以直接得到了

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    HashMap<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) {
        map.put(inorder[i], i);
    }
    return buildTreeHelper(preorder, 0, preorder.length, inorder, 0, inorder.length, map);
}

private TreeNode buildTreeHelper(int[] preorder, int p_start, int p_end, int[] inorder, int i_start, int i_end,
                                 HashMap<Integer, Integer> map) {
    if (p_start == p_end) {
        return null;
    }
    int root_val = preorder[p_start];
    TreeNode root = new TreeNode(root_val);
    int i_root_index = map.get(root_val);
    int leftNum = i_root_index - i_start;
    root.left = buildTreeHelper(preorder, p_start + 1, p_start + leftNum + 1, inorder, i_start, i_root_index, map);
    root.right = buildTreeHelper(preorder, p_start + leftNum + 1, p_end, inorder, i_root_index + 1, i_end, map);
    return root;
}

```

>再优化
用pre变量保存当前要构造的树的根节点，从根节点开始递归的构造左子树和右子树，in变量指向当前根节点可用数字的开头，然后对于当前pre有一个停止点stop，从in到stop表示要构造的树当前的数字范围。

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return buildTreeHelper(preorder,  inorder, (long)Integer.MAX_VALUE + 1);
}
int pre = 0;
int in = 0;
private TreeNode buildTreeHelper(int[] preorder, int[] inorder, long stop) {
    //到达末尾返回 null
    if(pre == preorder.length){
        return null;
    }
    //到达停止点返回 null
    //当前停止点已经用了，in 后移
    if (inorder[in] == stop) {
        in++;
        return null;
    }
    int root_val = preorder[pre++];
    TreeNode root = new TreeNode(root_val);   
    //左子树的停止点是当前的根节点
    root.left = buildTreeHelper(preorder,  inorder, root_val);
    //右子树的停止点是当前树的停止点
    root.right = buildTreeHelper(preorder, inorder, stop);
    return root;
}

```


