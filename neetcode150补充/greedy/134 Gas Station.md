# 134. Gas Station

>There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

>You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.

>Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique

## Solution 1 (一种神奇的解法)
https://leetcode.cn/problems/gas-station/solution/shi-yong-tu-de-si-xiang-fen-xi-gai-wen-ti-by-cyayc/
（见图 非常清晰明了的图解 很有意思）

时间复杂度：O(N)
空间复杂度：O(1)
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
    int len = gas.length;
    int spare = 0;
    int minSpare = Integer.MAX_VALUE;
    int minIndex = 0;

    for (int i = 0; i < len; i++) {
        spare += gas[i] - cost[i];
        if (spare < minSpare) {
            minSpare = spare;
            minIndex = i;
        }
    }

    return spare < 0 ? -1 : (minIndex + 1) % len;
}


}
//首先判断总gas能不能大于等于总cost，如果总gas不够，
//一切都白搭对吧（总（gas- cost）不用单独去计算，
//和找最低点时一起计算即可，只遍历一次）；

//再就是找总（gas-cost）的最低点，不管正负
//（当然如果最低点都是正的话那肯定能跑完了）；

//找到最低点后，如果有解，那么解就是最低点的下一个点，
//因为总（gas-cost）是大于等于0的，
//所以前面损失的gas我从最低点下一个点开始都会拿回来！
//（此处@小马哥！“我要争一口气，不是想证明我了不起。我是要告诉人家，我失去的东西一定要拿回来！”），别管后面的趋势是先加后减还是先减后加，最终结果我是能填平前面的坑的。
```

## solution 2 
https://leetcode.cn/problems/gas-station/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--30/
(见题解一步一步优化)
```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    int n = gas.length;
    for (int i = 0; i < n; i++) {
        int j = i;
        int remain = gas[i];
        while (remain - cost[j] >= 0) {
            //减去花费的加上新的点的补给
            remain = remain - cost[j] + gas[(j + 1) % n];
            j = (j + 1) % n;
            //j 回到了 i
            if (j == i) {
                return i;
            }
        }
        //最远距离绕到了之前，所以 i 后边的都不可能绕一圈了
        if (j < i) {
            return -1;
        }
        //i 直接跳到 j，外层 for 循环执行 i++，相当于从 j + 1 开始考虑
        i = j;

    }
    return -1;
}
```