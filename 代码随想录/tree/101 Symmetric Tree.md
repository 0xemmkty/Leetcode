# 101. Symmetric Tree

>Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).


递归实现
乍一看无从下手，但用递归其实很好解决。
根据题目的描述，镜像对称，就是左右两边相等，也就是左子树和右子树是相当的。
注意这句话，左子树和右子相等，也就是说要递归的比较左子树和右子树。
我们将根节点的左子树记做 left，右子树记做 right。比较 left 是否等于 right，不等的话直接返回就可以了。
如果相当，比较 left 的左节点和 right 的右节点，再比较 left 的右节点和 right 的左节点
比如看下面这两个子树(他们分别是根节点的左子树和右子树)，能观察到这么一个规律：
左子树 22 的左孩子 == 右子树 22 的右孩子
左子树 22 的右孩子 == 右子树 22 的左孩子


    2         2
   / \       / \
  3   4     4   3
 / \ / \   / \ / \
8  7 6  5 5  6 7  8
根据上面信息可以总结出递归函数的两个条件：
终止条件：

left 和 right 不等，或者 left 和 right 都为空
递归的比较 left，left 和 right.right，递归比较 left，right 和 right.left

作者：wang_ni_ma
链接：https://leetcode.cn/problems/symmetric-tree/solution/dong-hua-yan-shi-101-dui-cheng-er-cha-shu-by-user7/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## Solution 1 (DFS)
```java
class Solution {
	public boolean isSymmetric(TreeNode root) {
		if(root==null) {
			return true;
		}
		//调用递归函数，比较左节点，右节点
		return dfs(root.left,root.right);
	}
	
	boolean dfs(TreeNode left, TreeNode right) {
		//递归的终止条件是两个节点都为空
		//或者两个节点中有一个为空
		//或者两个节点的值不相等
		if(left==null && right==null) {
			return true;
		}
		if(left==null || right==null) {
			return false;
		}
		if(left.val!=right.val) {
			return false;
		}
		//再递归的比较 左节点的左孩子 和 右节点的右孩子
		//以及比较  左节点的右孩子 和 右节点的左孩子
		return dfs(left.left,right.right) && dfs(left.right,right.left);
	}
}


```

## Solution 2 (BFS)
```
队列实现
回想下递归的实现：
当两个子树的根节点相等时，就比较:
左子树的 left 和 右子树的 right，这个比较是用递归实现的。
现在我们改用队列来实现，思路如下：
首先从队列中拿出两个节点(left 和 right)比较
将 left 的 left 节点和 right 的 right 节点放入队列
将 left 的 right 节点和 right 的 left 节点放入队列
时间复杂度是 O(n)O(n)，空间复杂度是 O(n)O(n)
```
```java
class Solution {
	public boolean isSymmetric(TreeNode root) {
		if(root==null || (root.left==null && root.right==null)) {
			return true;
		}
		//用队列保存节点
		LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
		//将根节点的左右孩子放到队列中
		queue.add(root.left);
		queue.add(root.right);
		while(queue.size()>0) {
			//从队列中取出两个节点，再比较这两个节点
			TreeNode left = queue.removeFirst();
			TreeNode right = queue.removeFirst();
			//如果两个节点都为空就继续循环，两者有一个为空就返回false
			if(left==null && right==null) {
				continue;
			}
			if(left==null || right==null) {
				return false;
			}
			if(left.val!=right.val) {
				return false;
			}
			//将左节点的左孩子， 右节点的右孩子放入队列
			queue.add(left.left);
			queue.add(right.right);
			//将左节点的右孩子，右节点的左孩子放入队列
			queue.add(left.right);
			queue.add(right.left);
		}
		
		return true;
	}
}

```

 