# 102. Binary Tree Level Order Traversal

>Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

>Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]

>Solution 1 思路比较直接
## Solution 1 （BFS）
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<List<Integer>>();
        if (root == null) {
            return ret;
        }

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            List<Integer> level = new ArrayList<Integer>();
            int currentLevelSize = queue.size();
            for (int i = 1; i <= currentLevelSize; ++i) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            ret.add(level);
        }
        
        return ret;
    }
}
// review the question, the output is arraylist have arraylist
// therefore, when we use BFS. we need to think about how to build arraylist for each level.
// look at solution, we use tow arraylist: level and ret
// we use queuesize to calculate(or say store) the number of nodes in each level
//first, consider the null-root case
// then, create a new queue and put the root into it 
// if is not empty
// use for-loop and levelsize as boundary
// poll the node, and check its left and right
// be care that, the queue has how many node that it will cause the loop execute how many time.
//and each time the loop execute will check left and right
// remenber that queue is first-in-first-out, so the left and right which be add into the queue will not effect the node had already in queue
// what matter is only the levelsize val

```
>记树上所有节点的个数为 n。
时间复杂度：每个点进队出队各一次，故渐进时间复杂度为 O(n)。
空间复杂度：队列中元素的个数不超过 nn 个，故渐进空间复杂度为 O(n)。

## Solution 2 (DFS)
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
	public List<List<Integer>> levelOrder(TreeNode root) {
		if(root==null) {
			return new ArrayList<List<Integer>>();
		}
		//用来存放最终结果
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		dfs(1,root,res);
		return res;
	}
	
	void dfs(int index,TreeNode root, List<List<Integer>> res) {
		//假设res是[ [1],[2,3] ]， index是3，就再插入一个空list放到res中
		if(res.size()<index) {
			res.add(new ArrayList<Integer>());
		}
		//将当前节点的值加入到res中，index代表当前层，假设index是3，节点值是99
		//res是[ [1],[2,3] [4] ]，加入后res就变为 [ [1],[2,3] [4,99] ]
		res.get(index-1).add(root.val);
		//递归的处理左子树，右子树，同时将层数index+1
		if(root.left!=null) {
			dfs(index+1, root.left, res);
		}
		if(root.right!=null) {
			dfs(index+1, root.right, res);
		}
	}
}

```



```java
// 102.二叉树的层序遍历
class Solution {
    public List<List<Integer>> resList = new ArrayList<List<Integer>>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        //checkFun01(root,0);
        checkFun02(root);

        return resList;
    }

    //DFS--递归方式
    public void checkFun01(TreeNode node, Integer deep) {
        if (node == null) return;
        deep++;

        if (resList.size() < deep) {
            //当层级增加时，list的Item也增加，利用list的索引值进行层级界定
            List<Integer> item = new ArrayList<Integer>();
            resList.add(item);
        }
        resList.get(deep - 1).add(node.val);

        checkFun01(node.left, deep);
        checkFun01(node.right, deep);
    }

    //BFS--迭代方式--借助队列
    public void checkFun02(TreeNode node) {
        if (node == null) return;
        Queue<TreeNode> que = new LinkedList<TreeNode>();
        que.offer(node);

        while (!que.isEmpty()) {
            List<Integer> itemList = new ArrayList<Integer>();
            int len = que.size();

            while (len > 0) {
                TreeNode tmpNode = que.poll();
                itemList.add(tmpNode.val);

                if (tmpNode.left != null) que.offer(tmpNode.left);
                if (tmpNode.right != null) que.offer(tmpNode.right);
                len--;
            }

            resList.add(itemList);
        }

    }
}
```