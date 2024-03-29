# 1231. Divide Chocolate (dai)
题目地址
https://leetcode.com/problems/divide-chocolate/
题目描述
You have one chocolate bar that consists of some chunks. Each chunk has its own sweetness given by the array sweetness.

You want to share the chocolate with your K friends so you start cutting the chocolate bar into K+1 pieces using K cuts, each piece consists of some consecutive chunks.

Being generous, you will eat the piece with the minimum total sweetness and give the other pieces to your friends.

Find the maximum total sweetness of the piece you can get by cutting the chocolate bar optimally.

Example 1:
Input: sweetness = [1,2,3,4,5,6,7,8,9], K = 5
Output: 6
Explanation: You can divide the chocolate to [1,2,3], [4,5], [6], [7], [8], [9]

## solution
```java

class Solution {
  public int maximizeSweetness(int[] sweetness, int K) {
    int l = 0, r = Arrays.stream(sweetness).sum() / (K + 1);
    while (l < r) {
      int mid = l + (r - l + 1) / 2;
      int cnt = 0;
      int sum = 0;
      for (int i = 0; i < sweetness.length; i++) {
        sum += sweetness[i];
        if (sum >= mid) {
          sum = 0;
          cnt++;
        }
      }

      if (cnt >= K + 1) {
        l = mid;
      } else {
        r = mid - 1;            
      }
    }

    return l;
  }
}
```