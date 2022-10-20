# 155. Min Stack

>Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

>Implement the MinStack class:

>MinStack() initializes the stack object.
void push(int val) pushes the element val onto the stack.
void pop() removes the element on the top of the stack.
int top() gets the top element of the stack.
int getMin() retrieves the minimum element in the stack.
You must implement a solution with O(1) time complexity for each function.

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