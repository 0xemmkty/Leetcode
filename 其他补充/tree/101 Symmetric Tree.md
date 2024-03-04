101. Symmetric Tree
Solved
Easy
Topics
Companies
Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

```python
class Solution(object):
	def isSymmetric(self, root):
		"""
		:type root: TreeNode
		:rtype: bool
		"""
		if not root:
			return True
		def dfs(left,right):
			# 递归的终止条件是两个节点都为空
			# 或者两个节点中有一个为空
			# 或者两个节点的值不相等
			if not (left or right):
				return True
			if not (left and right):
				return False
			if left.val!=right.val:
				return False
			return dfs(left.left,right.right) and dfs(left.right,right.left)
		# 用递归函数，比较左节点，右节点
		return dfs(root.left,root.right)

```
```
为什么需要一个help function呢
看一个错误version：
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return False
        if root.right and root.left:
            return True
        if not (root.right and root.left):
            return False
        return self.isSymmetric(root.left.left, root.right.right) and self.isSymmetric(root.left.right, root.right.left)

这就是没有用help function的，并且你仔细想想就会知道哪里有问题
hint：root的left和right没有判断
so本题用的不是纯递归，相当于是dfs
```