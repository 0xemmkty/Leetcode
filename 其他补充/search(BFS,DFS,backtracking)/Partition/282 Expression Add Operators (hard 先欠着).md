# 282. Expression Add Operators (hard 先欠着)

Given a string num that contains only digits and an integer target, return all possibilities to insert the binary operators '+', '-', and/or '*' between the digits of num so that the resultant expression evaluates to the target value.

Note that operands in the returned expressions should not contain leading zeros.

```
Example 1:

Input: num = "123", target = 6
Output: ["1*2*3","1+2+3"]
Explanation: Both "1*2*3" and "1+2+3" evaluate to 6.
```

## Solution (backtracking)
https://leetcode.cn/problems/expression-add-operators/solution/gong-shui-san-xie-hui-su-suan-fa-yun-yon-nl9z/
```
回溯算法
最开始的想法是先使用 DFS 搜出来所有的表达式，然后套用 （题解）227. 基本计算器 II 方案，计算所有表达式的结果，并将计算结果为 targettarget 的表达式加到结果集。

假设原字符串 numnum 的长度为 nn，由于每个位置之间存在四种插入决策（不插入符号、+、- 和 *），共有 n - 1n−1 个位置需要决策，因此搜索所有表达式的复杂度为 O(4^{n - 1})O(4 
n−1
 )；同时需要对所有的表达式执行计算，复杂度为 O(n)O(n)，整体复杂度为 O(n * 4^{n - 1})O(n∗4 
n−1
 )。

添加运算符后的表达式长度不会超过 2020，因此总的计算量应该是在 10^710 
7
  以内，但可能是因为常数问题超时了（各种优化双栈操作也还是 TLE，在这上面浪费了好多时间 QWQ）。

因此，我们需要考虑在搜索过程中进行计算，以避免使用 （题解）227. 基本计算器 II 这种常数较大的计算方式。

我们考虑如果只有 + 和 - 的话，可以很容易将运算和回溯搜索所有表达进行结合。但当存在 * 时，由于存在运算优先级的问题，我们需要记录形如 a + b * c 中的乘法部分。

实现上，除了记录当前决策到原串 numnum 的哪一位 uu，以及当前的运算结果 curcur 以外，还需要额外记录最后一次的计算结果 prevprev，然后在决策表达式中的第 kk 个部分时，对本次添加的运算符合做分情况讨论：

如果本次添加的 + 操作，且第 kk 项的值是 nextnext：那么直接使用 cur + nextcur+next 来更新 curcur，同时 nextnext 作为下一次的 prevprev；
如果本次添加的 - 操作，且第 kk 项的值是 nextnext：同理，那么直接使用 cur - nextcur−next 来更新 curcur，同时 -next−next 作为下一次的 prevprev；
如果本次添加的 * 操作，且第 kk 项的值是 nextnext：此时需要考虑运算符的优先级问题，由于本次的 nextnext 是与上一次的操作数 prevprev 执行乘法，而 curcur 已经累加了 prevprev 的影响，因此需要先减去 prevprev，再加上 prev * nextprev∗next，以此来更新 curcur，同时 prev * nextprev∗next 也作为下一次的 prevprev。
一些细节：需要注意前导零（00 单独作为一位是被允许的，但是多位且首部为 00 是不允许的）以及 + 和 - 不作为一元运算符（运算符不能出现在表达式的首部）的情况。

```
```java
class Solution {
    List<String> ans = new ArrayList<>();
    String s;
    int n, t;
    public List<String> addOperators(String num, int target) {
        s = num;
        n = s.length();
        t = target;
        dfs(0, 0, 0, "");
        return ans;
    }
    void dfs(int u, long prev, long cur, String ss) {
        if (u == n) {
            if (cur == t) ans.add(ss);
            return ;
        }
        for (int i = u; i < n; i++) {
            if (i != u && s.charAt(u) == '0') break;
            long next = Long.parseLong(s.substring(u, i + 1));
            if (u == 0) {
                dfs(i + 1, next, next, "" + next);
            } else {
                dfs(i + 1,  next, cur + next, ss + "+" + next);
                dfs(i + 1, -next, cur - next, ss + "-" + next);
                long x = prev * next;
                dfs(i + 1, x, cur - prev + x, ss + "*" + next);
            }
        }
    }
}

```