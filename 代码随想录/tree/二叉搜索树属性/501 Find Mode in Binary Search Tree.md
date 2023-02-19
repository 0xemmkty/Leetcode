# 501. Find Mode in Binary Search Tree

>Given the root of a binary search tree (BST) with duplicates, return all the mode(s) (i.e., the most frequently occurred element) in it.
If the tree has more than one mode, return them in any order.
Assume a BST is defined as follows:
The left subtree of a node contains only nodes with keys less than or equal to the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.

## Solution 1

java
```java
class Solution {

    // 记录上一个节点值，初始值为null是为了判断当前节点是否是第一个节点
    private Integer pre = null;
    // 二叉树中节点最大出现次数
    private int maxCount = 0;
    // 当前节点的值的最大出现次数
    private int currCount = 0;
    // 众数的个数，也就是数组长度
    private int retCount = 0;
    // 存储众数的数组
    private int[] ret = null;

    public int[] findMode(TreeNode root) {
        inOrder(root);
        pre = null;
        currCount = 0;
        ret = new int[retCount];
        retCount = 0;
        inOrder(root);
        return ret;
    }

    private void inOrder(TreeNode root) {
        if (root == null) {
            return;
        }
        inOrder(root.left);
        // 重点：中序遍历，搜索二叉树增序，因此只需要比较当前和上一个的值
        // 当前节点值等于前一个节点的值，这个值的出现次数+1
        if (pre != null && root.val == pre) {
            currCount++;
        } else {
            // 如果当前节点是第一个节点或者当前节点值不等于前一个节点，那么这个值的出现次数记为1
            currCount = 1;
        }
        // 如果当前值的出现次数比最大出现次数还要大，那么说明当前值才是出现最多的众数，因此数组个数记为1
        if (currCount > maxCount) {
            maxCount = currCount;
            retCount = 1;
        } else if (currCount == maxCount) {
            // 这一步是为了第二次调用时给数组赋值用的
            // 第一次调用计算得出了最大出现次数，那么第二次调用时，发现当前值的最大出现次数==第一次得出的最大出现次数，那么这个值肯定是众数之一
            if (ret != null) {
                ret[retCount] = root.val;
            }
            // 如果当前值的出现次数等于最大出现次数，那么说明有多个众数，数组个数+1
            retCount++;
        }
        // 更新pre值
        pre = root.val;
        inOrder(root.right);
    }
}
```

```java
class Solution {
    ArrayList<Integer> resList;
    int maxCount;
    int count;
    TreeNode pre;

    public int[] findMode(TreeNode root) {
        resList = new ArrayList<>();
        maxCount = 0;
        count = 0;
        pre = null;
        findMode1(root);
        int[] res = new int[resList.size()];
        for (int i = 0; i < resList.size(); i++) {
            res[i] = resList.get(i);
        }
        return res;
    }

    public void findMode1(TreeNode root) {
        if (root == null) {
            return;
        }
        findMode1(root.left);

        int rootValue = root.val;
        // 计数
        if (pre == null || rootValue != pre.val) {
            count = 1;
        } else {
            count++;
        }
        // 更新结果以及maxCount
        if (count > maxCount) {
            resList.clear();
            resList.add(rootValue);
            maxCount = count;
        } else if (count == maxCount) {
            resList.add(rootValue);
        }
        pre = root;

        findMode1(root.right);
    }
}
```

python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.pre = TreeNode()
        self.count = 0
        self.max_count = 0
        self.result = []

    def findMode(self, root: TreeNode) -> List[int]:
        if not root: return None
        self.search_BST(root)
        return self.result
        
    def search_BST(self, cur: TreeNode) -> None:
        if not cur: return None
        self.search_BST(cur.left)
        # 第一个节点
        if not self.pre:
            self.count = 1
        # 与前一个节点数值相同
        elif self.pre.val == cur.val:
            self.count += 1 
        # 与前一个节点数值不相同
        else:
            self.count = 1
        self.pre = cur

        if self.count == self.max_count:
            self.result.append(cur.val)
        
        if self.count > self.max_count:
            self.max_count = self.count
            self.result = [cur.val]	# 清空self.result，确保result之前的的元素都失效
        
        self.search_BST(cur.right)
```
## Solution 2 (迭代)
java
```java
class Solution {
    public int[] findMode(TreeNode root) {
        TreeNode pre = null;
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> result = new ArrayList<>();
        int maxCount = 0;
        int count = 0;
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            if (cur != null) {
                stack.push(cur);
                cur =cur.left;
            }else {
                cur = stack.pop();
                // 计数
                if (pre == null || cur.val != pre.val) {
                    count = 1;
                }else {
                    count++;
                }
                // 更新结果
                if (count > maxCount) {
                    maxCount = count;
                    result.clear();
                    result.add(cur.val);
                }else if (count == maxCount) {
                    result.add(cur.val);
                }
                pre = cur;
                cur = cur.right;
            }
        }
        return result.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

python
```python
class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        stack = []
        cur = root
        pre = None
        maxCount, count = 0, 0
        res = []
        while cur or stack:
            if cur:  # 指针来访问节点，访问到最底层
                stack.append(cur)
                cur = cur.left
            else:  # 逐一处理节点
                cur = stack.pop()
                if pre == None:  # 第一个节点
                    count = 1
                elif pre.val == cur.val:  # 与前一个节点数值相同
                    count += 1
                else:
                    count = 1
                if count == maxCount:
                    res.append(cur.val)
                if count > maxCount:
                    maxCount = count
                    res.clear()
                    res.append(cur.val)

                pre = cur
                cur = cur.right
        return res	
```