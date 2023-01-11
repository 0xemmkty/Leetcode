# 122. Best Time to Buy and Sell Stock II

>You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

>On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

>Find and return the maximum profit you can achieve.

示例 1:

输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4。随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。


## S1 （贪心）
>**重点** 只收集每天正利润
如果想到其实最终利润是可以分解的，那么本题就很容易了！

如何分解呢？

假如第0天买入，第3天卖出，那么利润为：prices[3] - prices[0]。

相当于(prices[3] - prices[2]) + (prices[2] - prices[1]) + (prices[1] - prices[0])。

此时就是把利润分解为每天为单位的维度，而不是从0天到第3天整体去考虑！

那么根据prices可以得到每天的利润序列：(prices[i] - prices[i - 1]).....(prices[1] - prices[0])。

从图中可以发现，其实我们需要收集每天的正利润就可以，收集正利润的区间，就是股票买卖的区间，而我们只需要关注最终利润，不需要记录区间。

>那么**只收集正利润**就是贪心所贪的地方！

局部最优：收集每天的正利润，全局最优：求得最大利润。

举个例子：
股票价格：7 1 5 10 3 6 4
每天利润：-6 4 5 -7 3 -2
贪心，只收集每天正利润： 4+5+3=12

时间复杂度：O(n)
空间复杂度：O(1)

 ```java
 // 贪心思路
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0;
        for (int i = 1; i < prices.length; i++) {
            result += Math.max(prices[i] - prices[i - 1], 0);
        }
        return result;
    }
}
```