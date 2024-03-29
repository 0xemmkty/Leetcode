# 155. Min Stack

>Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

>Implement the MinStack class:

>MinStack() initializes the stack object.
void push(int val) pushes the element val onto the stack.
void pop() removes the element on the top of the stack.
int top() gets the top element of the stack.
int getMin() retrieves the minimum element in the stack.
You must implement a solution with O(1) time complexity for each function.

## 辅助栈
```python
# 注意self，pop地方要val=pop（）， 理解意义
class MinStack(object):

    def __init__(self):
        self.stack = []
        self.minStack = []

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.stack.append(val)
        if not self.minStack or val <= self.minStack[-1]:
            self.minStack.append(val)
        

    def pop(self):
        """
        :rtype: None
        """
        val = self.stack.pop()
        if self.minStack[-1] == val:
            self.minStack.pop()
        

    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1]

        

    def getMin(self):
        """
        :rtype: int
        """
        return self.minStack[-1]
        


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```
## Solution 1 (use just one stack)
```
解题思路
题目要求在常数时间内获得栈中的最小值，因此不能在 getMin() 的时候再去计算最小值，最好应该在 push 或者 pop 的时候就已经计算好了当前栈中的最小值。

前排的众多题解中，基本都讲了「辅助栈」的概念，这是一种常见的思路，但是有没有更容易懂的方法呢？

可以用一个栈，这个栈同时保存的是每个数字 x 进栈的时候的值 与 插入该值后的栈内最小值。即每次新元素 x 入栈的时候保存一个元组：（当前值 x，栈内最小值）。

这个元组是一个整体，同时进栈和出栈。即栈顶同时有值和栈内最小值，top()函数是获取栈顶的当前值，即栈顶元组的第一个值； getMin() 函数是获取栈内最小值，即栈顶元组的第二个值；pop() 函数时删除栈顶的元组。

每次新元素入栈时，要求新的栈内最小值：比较当前新插入元素 x 和 当前栈内最小值（即栈顶元组的第二个值）的大小。

新元素入栈：当栈为空，保存元组 (x, x)；当栈不空，保存元组 (x, min(此前栈内最小值， x)))
出栈：删除栈顶的元组。
```

```java
class MinStack {

    // 数组栈, [当前值, 当前最小值]
    private Stack<int[]> stack = new Stack<>();

    public MinStack() {

    }

    public void push(int x) {
        if (stack.isEmpty()){
            stack.push(new int[]{x, x});
        }else {
            stack.push(new int[]{x, Math.min(x, stack.peek()[1])});
        }
    }

    public void pop() {
        stack.pop();
    }

    public int top() {
        return stack.peek()[0];
    }

    public int getMin() {
        return stack.peek()[1];
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */

 // interesting to use just one stack
 // and the pair to store the value and the mini value in the stack
 ```

 ## Solution 2 (use self-defi node) faster runtime that the solution 1
 ```java
 /**
 * 通过自定义链表实现，每个节点存储当前值，当前最小值，和它前面的节点
 * 执行用时：5 ms, 在所有 Java 提交中击败了99.84%的用户
 * 内存消耗：40.4 MB, 在所有 Java 提交中击败了47.08%的用户
 * */
class MinStack {

    private Node node;

    public MinStack() {

    }

    public void push(int x) {
        if (node == null){
            node = new Node(x, x);
        }else {
            // node = new Node(x, Math.min(x, node.min), node);
            Node next = new Node(x, Math.min(x, node.min));
            next.prev = node;
            node = next;
        }
    }

    public void pop() {
        node = node.prev;
    }

    public int top() {
        return node.val;
    }

    public int getMin() {
        return node.min;
    }

    private class Node {
        int val;
        int min;
        Node prev;

        private Node(int val, int min){
            this.val = val;
            this.min = min;
        }

        private Node(int val, int min, Node prev){
            this.val = val;
            this.min = min;
            this.prev = prev;
        }
    }
}
```