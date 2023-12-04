107. Binary Tree Level Order Traversal II
Medium
4.7K
318
Companies
Given the root of a binary tree, return the bottom-up level order traversal of its nodes' values. (i.e., from left to right, level by level from leaf to root).

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        res,queue = [],[root]
        while queue:
            size=len(queue)
            level=[]
            for _ in range(size):
                # 弹出并移除
                node = queue.pop(0)
                level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            # insert(0) 插入到第一个
            res.insert(0,level)
        return res
                

        
```