## 138. Copy List with Random Pointer

>A linked list of length n is given such that each node contains an additional **random pointer**, which could **point to any node in the list, or null**.

>Construct **a deep copy of the list**. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

>For example, if there are two nodes X and Y in the original list, where X.random --> Y, then for the corresponding two nodes x and y in the copied list, x.random --> y.

>Return the head of the copied linked list.

>The linked list is represented in the input/output as a list of n nodes. Each node is represented as a pair of [val, random_index] where:

>val: an integer representing Node.val
random_index: the index of the node (range from 0 to n-1) that the random pointer points to, or null if it does not point to any node.
Your code will only be given the head of the original linked list.


link is useful
https://leetcode.cn/problems/copy-list-with-random-pointer/solution/liang-chong-shi-xian-tu-jie-138-fu-zhi-dai-sui-ji-/
these two solution are both interesting!

## Solution 1 
important point to solve this problem is to copy the random pointer
this solution use a interesting way:
let the copied node be the next node 
than the problem can be solved
```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        if(head==null) {
            return null;
        }
        Node p = head;
        //第一步，在每个原节点后面创建一个新节点
        //1->1'->2->2'->3->3'
        while(p!=null) {
            Node newNode = new Node(p.val);
            newNode.next = p.next;
            p.next = newNode;
            p = newNode.next;
        }
        p = head;
        //第二步，设置新节点的随机节点
        while(p!=null) {
            if(p.random!=null) {
                // beaytiful step
                p.next.random = p.random.next;
            }
            p = p.next.next;
        }
        Node dummy = new Node(-1);
        p = head;
        Node cur = dummy;
        //第三步，将两个链表分离
        while(p!=null) {
            cur.next = p.next;
            cur = cur.next;
            p.next = cur.next;
            p = p.next;
        }
        return dummy.next;
    }
}	
```

## Solution 2
我们用哈希表来解决这个问题
首先创建一个哈希表，再遍历原链表，遍历的同时再不断创建新节点
我们将原节点作为key，新节点作为value放入哈希表中

从上图中我们可以发现，原节点和新节点是一一对应的关系，所以

map.get(原节点)，得到的就是对应的新节点
map.get(原节点.next)，得到的就是对应的新节点.next
map.get(原节点.random)，得到的就是对应的新节点.random
所以，我们只需要再次遍历原链表，然后设置：
新节点.next -> map.get(原节点.next)
新节点.random -> map.get(原节点.random)
这样新链表的next和random都被串联起来了
最后，我们然后map.get(head)，也就是对应的新链表的头节点，就可以解决此问题了。

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        if(head==null) {
            return null;
        }
        //创建一个哈希表，key是原节点，value是新节点
        Map<Node,Node> map = new HashMap<Node,Node>();
        Node p = head;
        //将原节点和新节点放入哈希表中
        while(p!=null) {
            Node newNode = new Node(p.val);
            map.put(p,newNode);
            p = p.next;
        }
        p = head;
        //遍历原链表，设置新节点的next和random
        while(p!=null) {
            Node newNode = map.get(p);
            //p是原节点，map.get(p)是对应的新节点，p.next是原节点的下一个
            //map.get(p.next)是原节点下一个对应的新节点
            if(p.next!=null) {
                newNode.next = map.get(p.next);
            }
            //p.random是原节点随机指向
            //map.get(p.random)是原节点随机指向  对应的新节点 
            if(p.random!=null) {
                newNode.random = map.get(p.random);
            }
            p = p.next;
        }
        //返回头结点，即原节点对应的value(新节点)
        return map.get(head);
    }
}

```
 