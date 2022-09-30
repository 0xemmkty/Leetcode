## 2096. Step-By-Step Directions From a Binary Tree Node to Another

>You are given the root of a binary tree with n nodes. Each node is uniquely assigned a value from 1 to n. You are also given an integer startValue representing the value of the start node s, and a different integer destValue representing the value of the destination node t.

>Find the shortest path starting from node s and ending at node t. Generate step-by-step directions of such path as a string consisting of only the uppercase letters 'L', 'R', and 'U'. Each letter indicates a specific direction:

>- 'L' means to go from a node to its left child node.
>- 'R' means to go from a node to its right child node.
>- 'U' means to go from a node to its parent node.
Return the step-by-step directions of the shortest path from node s to node t.

## Solution 1 (DFS)
二叉树中一个节点到另一个节点的最短路径一定可以分为两个部分（可能为空）：从起点节点向上到两个节点的最近公共祖先，再从最近公共祖先向下到达终点节点。

思路与算法

根据 提示 11 与 提示 22，我们首先需要找到 ss 与 tt，并维护根节点到 ss 与 tt 的最短路径对应方向字符串 \textit{path}_1path 
1
​
  与 \textit{path}_2path 
2
​
 。随后，我们计算两个字符串的最长公共前缀，并在 \textit{path}_1path 
1
​
  与 \textit{path}_2path 
2
​
  中截去这部分前缀。随后，由于 ss 到最近公共祖先的路径是向上的，因此我们将处理后的 \textit{path}_1path 
1
​
  中的所有字符均修改成代表向上的 \texttt{`U'}‘U’，并将处理后的 \textit{path}_2path 
2
​
  拼接至尾部，最终得到的字符串即为待求 ss 到 tt 每一步的最短路径。

为了求出 \textit{path}_1path 
1
​
  与 \textit{path}_2path 
2
​
 ，一种方法是自根节点向下进行深度优先搜索，对于每个节点均更新并保存根节点到该节点的路径字符串，待找到目标节点后返回对应的字符串。但对于树为一条链的情况，这种方法的时空复杂度均为 O(n^2)O(n 
2
 )，不符合题目要求。因此，我们需要尝试从 ss 与 tt 向上遍历直到根节点从而得到对应的路径，并反向得出目标字符串。但对于一般的二叉树数据结构，我们无法很快地得到某一个节点的父节点，因此我们需要通过深度优先搜索，利用哈希表额外维护每个节点对应的父节点。维护父节点哈希表后，我们便可以在 O(n)O(n) 的时间与空间复杂度内求出 \textit{path}_1path 
1
​
  与 \textit{path}_2path 
2
​
 。

具体地，我们首先利用函数 \textit{dfs}(\textit{curr})dfs(curr) 自根节点对二叉树进行深度优先搜索，从而得到 ss 与 tt，以及存储每个节点父节点的哈希表 \textit{fa}fa。随后，我们利用函数 \textit{path}(\textit{curr})path(curr) 求出根节点到 ss 与 tt 的路径对应字符串 \textit{path}_1path 
1
​
  与 \textit{path}_2path 
2
​
 。在函数 \textit{path}(\textit{curr})path(curr) 中，我们维护路径字符串 \textit{res}res，并利用哈希表 \textit{fa}fa 逐层向上遍历，每当求出当前节点 \textit{curr}curr 的父节点 \textit{par}par 时，我们判断 \textit{curr}curr 是 \textit{par}par 的左孩子还是右孩子节点，并在路径字符串的末尾添加上对应的 \texttt{`L'}‘L’ 或 \texttt{`R'}‘R’；最终我们将 \textit{res}res 反转，即为待求的路径对应字符串。最后，我们计算两个路径字符串的最长公共前缀并删去，同时将 \textit{path}_1path 
1
​
  的所有字符改为表示向上的 \texttt{`U'}‘U’，并与截去前缀的 \textit{path}_2path 
2
​
  拼接作为 ss 到 tt 最短路径每一步的方向返回。

```java
class Solution {
    StringBuilder res = new StringBuilder();
    StringBuilder start = new StringBuilder();
    StringBuilder dest = new StringBuilder();
    public String getDirections(TreeNode root, int startValue, int destValue) {
        TreeNode ancestor = findRoot(root, startValue, destValue);        
        dfsStart(ancestor, startValue);
        dfsDest(ancestor, destValue);
        return res.toString();
    }
    public void dfsStart(TreeNode root, int startValue){
        if(root == null){
            return;
        }
        if(root.val==startValue){
            res.append(start);
            return;
        }
        start.append("U");
        dfsStart(root.left, startValue);
        start.deleteCharAt(start.length()-1);
        start.append("U");
        dfsStart(root.right, startValue);
        start.deleteCharAt(start.length()-1);
    }

    public void dfsDest(TreeNode root, int destValue){
        if(root == null){
            return;
        }    
        if(root.val == destValue){
            res.append(dest);
            return;
        }
        dest.append("L");
        dfsDest(root.left, destValue);
        dest.deleteCharAt(dest.length()-1);
        dest.append("R");
        dfsDest(root.right, destValue);
        dest.deleteCharAt(dest.length()-1);
    }

   public TreeNode findRoot(TreeNode root, int startValue, int destValue){
        if(root == null || root.val == startValue || root.val == destValue) return root;
        TreeNode left = findRoot(root.left, startValue, destValue);
        TreeNode right = findRoot(root.right, startValue, destValue);
        if(left == null) return right;
        if(right == null) return left;
        return root;
    }
}
```

```java
private boolean find(TreeNode n, int val, StringBuilder sb) {
    if (n.val == val) 
        return true;
    if (n.left != null && find(n.left, val, sb))
        sb.append("L");
    else if (n.right != null && find(n.right, val, sb))
        sb.append("R");
    return sb.length() > 0;
}
public String getDirections(TreeNode root, int startValue, int destValue) {
    StringBuilder s = new StringBuilder(), d = new StringBuilder();
    find(root, startValue, s);
    find(root, destValue, d);
    int i = 0, max_i = Math.min(d.length(), s.length());
    while (i < max_i && s.charAt(s.length() - i - 1) == d.charAt(d.length() - i - 1))
        ++i;
    return "U".repeat(s.length() - i) + d.reverse().toString().substring(i);
}
```

```java
class Solution {
    public String getDirections(TreeNode root, int startValue, int destValue) {
        // get the top down path along from the root
        StringBuilder sb1 = new StringBuilder();
        StringBuilder sb2 = new StringBuilder();
        getTopDownPath(root, startValue, sb1);
        getTopDownPath(root, destValue, sb2);
        
        // cut down the unnecessary path from root to lca
        int commonPathLen = getCommonPathLen(sb1, sb2);
        
        return "U".repeat(sb1.length() - commonPathLen) + sb2.substring(commonPathLen);
    }
    
    // return if the val node is found or not along the path from root
    public boolean getTopDownPath(TreeNode root, int val, StringBuilder sb) {
        if(root == null) return false;
        if(root.val == val) return true;        
        
        sb.append("L");
        boolean foundLeft = getTopDownPath(root.left, val, sb);
        if(!foundLeft) sb.setLength(sb.length() - 1);
        else return true;
        
        sb.append("R");
        boolean foundRight = getTopDownPath(root.right, val, sb);
        if(!foundRight) sb.setLength(sb.length() - 1);
        else return true;
        
        return false;
    }
    
    public int getCommonPathLen(StringBuilder sb1, StringBuilder sb2) {
        int i = 0;
        while(i < sb1.length() && i < sb2.length()) {
            if(sb1.charAt(i) == sb2.charAt(i)) {
                i++;
            } else {
                break;
            }
        }
        
        return i;
    }
}
```