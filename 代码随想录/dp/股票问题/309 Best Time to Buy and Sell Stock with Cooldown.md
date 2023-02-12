# 309. Best Time to Buy and Sell Stock with Cooldown

>You are given an array prices where prices[i] is the price of a given stock on the ith day.
Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:
After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 
```
Example 1:

Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

## Solution 1(dp)
前言：不要关注冷冻期！不要关注冷冻期！不要关注冷冻期！
只关注卖出的那一天！只关注卖出的那一天！只关注卖出的那一天！
题目中定义的“冷冻期”=卖出的那一天的后一天，题目设置冷冻期的意思是，如果昨天卖出了，今天不可买入，那么关键在于哪一天卖出，只要在今天想买入的时候判断一下前一天是不是刚卖出，即可，所以关键的一天其实是卖出的那一天，而不是卖出的后一天

因为当天卖出股票实际上也是属于“不持有”的状态，那么第i天如果不持有，那这个“不持有”就有了两种状态：1.本来就不持有，指不是因为当天卖出了才不持有的；2.第i天因为卖出了股票才变得不持有

而持有股票依旧只有一种状态

所以对于每一天i，都有可能是三种状态：
0.不持股且当天没卖出,定义其最大收益dp[i][0];
1.持股,定义其最大收益dp[i][1]；
2.不持股且当天卖出了，定义其最大收益dp[i][2]；

初始化：
dp[0][0]=0;//本来就不持有，啥也没干
dp[0][1]=-1*prices[0];//第0天只买入
dp[0][2]=0;//可以理解成第0天买入又卖出，那么第0天就是“不持股且当天卖出了”这个状态了，其收益为0，所以初始化为0是合理的

重头戏：

一、第i天不持股且没卖出的状态dp[i][0]，也就是我没有股票，而且还不是因为我卖了它才没有的，那换句话说是从i-1天到第i天转移时，它压根就没给我股票！所以i-1天一定也是不持有，那就是不持有的两种可能：i-1天不持股且当天没有卖出dp[i-1][0]；i-1天不持股但是当天卖出去了dp[i-1][2]；
所以： dp[i][0]=max(dp[i-1][0],dp[i-1][2])

二、第i天持股dp[i][1]，今天我持股，来自两种可能：
1、要么是昨天我就持股，今天继承昨天的，也就是dp[i-1][1]，这种可能很好理解；
2、要么：是昨天我不持股，今天我买入的，但前提是昨天我一定没卖！因为如果昨天我卖了，那么今天我不能交易！也就是题目中所谓“冷冻期”的含义，只有昨天是“不持股且当天没卖出”这个状态，我今天才能买入！所以是dp[i-1][0]-p[i]
所以： dp[i][1]=max(dp[i-1][1],dp[i-1][0]-p[i])

三、i天不持股且当天卖出了，这种就简单了，那就是说昨天我一定是持股的，要不然我今天拿什么卖啊，而持股只有一种状态，昨天持股的收益加上今天卖出得到的新收益，就是dp[i-1][1]+p[i]啦
所以：dp[i][2]=dp[i-1][1]+p[i]

总结：最后一天的最大收益有两种可能，而且一定是“不持有”状态下的两种可能，把这两种“不持有”比较一下大小，返回即可

作者：jin-ai-yi
链接：https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/solution/fei-zhuang-tai-ji-de-dpjiang-jie-chao-ji-tong-su-y/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```c
class Solution { public: int maxProfit(vector<int>& prices) {

    vector<vector<int>>dp(prices.size(),vector<int>(4));
    dp[0][0]=0;//不持有股票，没卖出的
    dp[0][1]=0;//不持有股票，卖出去了
    dp[0][2]=-1*prices[0];//持有股票，今天买入；
    dp[0][3]=-1*prices[0];//持有股票，非今天买入的；
    for(int i=1;i<prices.size();i++){
        dp[i][0]=max(dp[i-1][0],dp[i-1][1]);//前一天不持有股票的两种情况的最大值
        dp[i][1]=max(dp[i-1][2],dp[i-1][3])+prices[i];//今天卖出股票，来着前一天持有股票的最大值+pr
        dp[i][2]=dp[i-1][0]-prices[i];//今天买入股票，这前一天一定没有卖出股票
        dp[i][3]=max(dp[i-1][2],dp[i-1][3]);//今天没买股票，却持有股票，前一天继承来的,有两种情况
    }
    return max(dp[prices.size()-1][0],dp[prices.size()-1][1]);
    // 最后一天的最大收益有两种可能，而且一定是“不持有”状态下的两种可能，
    // 把这两种“不持有”比较一下大小
}
};
```
```java
class Solution {
    public int maxProfit(int[] prices) {
        int days = prices.length;
        // 定义动态规划状态变量，第一维记录天数，第二维记录股票的状态
        int[][] dp= new int[days][4];

        // 初始化状态变量
        // 状态0，表示不持有股票，且没卖出，意思就是本来就不持有，没做任何买卖
        dp[0][0] = 0;
        // 状态1，表示不持有股票，原因是今天卖出了（这里因为是第0天，所以没买入，所以收益也是0）
        dp[0][1] = 0;
        // 状态2，表示持有股票，今天买入的，因为是买入，所以收入是负的，
        dp[0][2] = -1 * prices[0];
        // 状态3，表示持有股票，非今天买入，从前一天继承过来的（这里因为是第0天，所以相当于是今天买入，今天卖出）
        dp[0][3] = -1 * prices[0];

        for (int i = 1; i < days; i++) {
            // 第i天不持有且本来也不持有，那说明前一天也不持有，即使持有也卖出了，那就是0和1两种情况
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1]);
            // 第i天不持有，因为今天卖出了，那收益就是今天的股票价格加上 之前累加的
            dp[i][1] = Math.max(dp[i-1][2], dp[i-1][3]) + prices[i];
            // 第i天持有，而且是今天买入（排除状态2、3），那么根据冷冻期 昨天一定没有卖出今天才能买入
            dp[i][2] = dp[i-1][0] - prices[i];
            // 第i天持有，且非今天买入, 那就是之前买入，计算之前累加的
            dp[i][3] = Math.max(dp[i-1][2], dp[i-1][3]);
        }

        //因为买卖到最后，一定是不持有的（即使亏了，卖也比不卖强），所以应该是0和1两种状态，取较大值
        return Math.max(dp[days-1][0], dp[days-1][1]);
    }
}
```