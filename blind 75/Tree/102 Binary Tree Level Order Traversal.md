# 102. Binary Tree Level Order Traversal

>Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

>Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]

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
