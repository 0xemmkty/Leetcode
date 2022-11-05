# 84. Largest Rectangle in Histogram (not really soved)

>Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.
```

## Solution 1 (single stack)
```java
class Solution {
   public int largestRectangleArea(int[] heights) {
        int res = 0;
        Deque<Integer> stack = new ArrayDeque<>();
        int[] new_heights = new int[heights.length + 2];
        for (int i = 1; i < heights.length + 1; i++) {
            new_heights[i] = heights[i - 1];
        }
        for (int i = 0; i < new_heights.length; i++) {
            while (!stack.isEmpty() && new_heights[stack.peek()] > new_heights[i]) {
                int cur = stack.pop();
                int l = stack.peek();
                int r = i;
                res = Math.max(res, (r - l - 1) * new_heights[cur]);
            }
            stack.push(i);
        }
        return res;
    }
}
// pop: get and remove
// peek: get without remove  
// attention: the thing put into stack is index!! not the value(height)
// why let the stack always have two "0"? to meet the "!stack.isEmpty" requirement
// first pop, then peek
// for and while structure
```
优化：
```JAVA
class Solution {
    public int largestRectangleArea(int[] heights) {
        Deque<Integer> stk = new ArrayDeque();
        int ret = 0;
        for (int i = 0; i <= heights.length; i++){
            while (!stk.isEmpty() && (i == heights.length || heights[i] < heights[stk.peek()])){
                int ind = stk.pop();
                int lastInd = stk.isEmpty() ? -1 : stk.peek();
                ret = Math.max(ret, heights[ind] * (i - lastInd - 1));
            }
            if (i < heights.length) stk.push(i);
        }
        return ret;
    }
}
```


```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        /*
        只做单调栈思路:参考"编程狂想曲"思路比较好理解
        1.核心思想:求每条柱子可以向左右延伸的长度->矩形最大宽度;矩形的高->柱子的高度
            计算以每一根柱子高度为高的矩形面积,维护面积最大值
        2.朴素的想法:遍历每一根柱子的高度然后向两边进行扩散找到最大宽度
        3.单调栈优化:因为最终的目的是寻找对应柱子height[i]右边首个严格小于height[i]的柱子height[r]
            左边同理找到首个严格小于height[i]的柱子height[l]
            维护一个单调递增栈(栈底->栈顶),那么每当遇到新加入的元素<栈顶便可以确定栈顶柱子右边界
            而栈顶柱子左边界就是栈顶柱子下面的柱子(<栈顶柱子)
            左右边界确定以后就可以进行面积计算与维护最大面积
        时间复杂度:O(N),空间复杂度:O(N)
        */
        // 引入哨兵
        // 哨兵的作用是 将最后的元素出栈计算面积 以及 将开头的元素顺利入栈
        // len为引入哨兵后的数组长度
        int len = heights.length + 2;
        int[] newHeight = new int[len];
        newHeight[0] = newHeight[len - 1] = 0;
        // [1,2,3]->[0,1,2,3,0]
        for(int i = 1; i < len - 1; i++) {
            newHeight[i] = heights[i - 1];
        }
        // 单调递增栈:存储每个柱子的索引,使得这些索引对应的柱子高度单调递增
        Stack<Integer> stack = new Stack<>();
        // 最大矩形面积
        int res = 0;
        // 遍历哨兵数组
        for(int i = 0; i < len; i++) {
            // 栈不为空且当前柱子高度<栈顶索引对应的柱子高度
            // 说明栈顶元素的右边界已经确定,就是索引为i的柱子(不含)
            // 此时将栈顶元素出栈,栈顶矩形左边界为栈顶元素下面的索引(首个小于栈顶)
            while(!stack.empty() && newHeight[i] < newHeight[stack.peek()]) {
                // 栈顶索引出栈并记录
                int pop = stack.pop();
                // 计算出栈顶元素矩形的宽度如(0,1,2)->[1,2,1],两边都不包含
                // 因此右索引-左索引-1=矩形宽度
                int w = i - stack.peek() - 1;
                // 栈顶索引对应的柱子高度就是矩形的高度
                int h = newHeight[pop];
                // 计算矩形面积
                int area = w * h;
                // 维护矩形面积最大值
                res = Math.max(res, area);
            }
            // 每当弹出一个索引就计算一个矩形面积
            // 直到当前元素>=栈顶元素(或者栈为空)时,栈顶柱子的右边界还没确定
            // 因此当前元素索引入栈即可
            stack.push(i);
        }
        return res;
    }
}
```

single increse stack 模版（c++）
```
stack<int> st;
for(int i = 0; i < nums.size(); i++)
{
	while(!st.empty() && st.top() > nums[i])
	{
		st.pop();
	}
	st.push(nums[i]);
}

作者：ikaruga
链接：https://leetcode.cn/problems/largest-rectangle-in-histogram/solution/84-by-ikaruga/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```