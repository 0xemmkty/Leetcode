# 11. Container With Most Water

>You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).
Find two lines that together with the x-axis form a container, such that the container contains the most water.
Return the maximum amount of water a container can store.
Notice that you may not slant the container.


 # Solution 1 (two pointer)

 >设两指针 i , j ，指向的水槽板高度分别为 h[i] ,h[j] ，此状态下水槽面积为S(i,j) 。由于可容纳水的高度由两板中的 短板 决定，因此可得如下 面积公式 ：
**S(i,j)=min(h[i],h[j])×(j−i)**
在每个状态下，无论长板或短板向中间收窄一格，都会导致水槽 底边宽度 -1−1​ 变短：
若向内 移动短板 ，水槽的短板min(h[i],h[j]) 可能变大，因此下个水槽的面积 可能增大 。
若向内 移动长板 ，水槽的短板min(h[i],h[j])​ 不变或变小，因此下个水槽的面积 一定变小 。**(因为是面积是由短板决定的)**
因此，初始化双指针分列水槽左右两端，循环每轮将短板向内移动一格，并更新面积最大值，直到两指针相遇时跳出；即可获得最大面积。

>算法流程：
初始化： 双指针 ii , jj 分列水槽左右两端；
循环收窄： 直至双指针相遇时跳出；
更新面积最大值 resres ；
选定两板高度中的**短板，向中间收窄一格**；
返回值： 返回面积最大值 res 即可；

>**复杂度分析**：
时间复杂度 O(N)​： 双指针遍历一次底边宽度 N。
空间复杂度 O(1)​： 变量 i , j, res 使用常数额外空间。

```java

    class Solution {
    public int maxArea(int[] height) {
        int i = 0, j = height.length - 1, res = 0;
        while(i < j) {
            res = height[i] < height[j] ? 
                Math.max(res, (j - i) * height[i++]): 
                Math.max(res, (j - i) * height[j--]); 
        }
        return res;
    }
    }
    // 1. set two pointer
    // 2. find the lower one
    // 3. the meaning of the res(S) 
    // 4. just the lower one change the value (the reason analyse above)
    // 5. the THREE LINE of res is very interesting
```


作者：jyd
链接：https://leetcode.cn/problems/container-with-most-water/solution/container-with-most-water-shuang-zhi-zhen-fa-yi-do/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。