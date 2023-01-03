# 93. Restore IP Addresses

>A valid IP address consists of exactly four integers separated by single dots. Each integer is between 0 and 255 (inclusive) and cannot have leading zeros.

>For example, "0.1.2.201" and "192.168.1.1" are valid IP addresses, but "0.011.255.245", "192.168.1.312" and "192.168@1.1" are invalid IP addresses.
Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s. You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.

 ```
 Example 1:

Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
```

## Solution 1(backtracking)
```
1、一开始，字符串的长度小于 4 或者大于 12 ，一定不能拼凑出合法的 ip 地址（这一点可以一般化到中间结点的判断中，以产生剪枝行为）；

2、每一个结点可以选择截取的方法只有 3 种：截 1 位、截 2 位、截 3 位，因此每一个结点可以生长出的分支最多只有 3 条分支；

根据截取出来的字符串判断是否是合理的 ip 段，这里写法比较多，可以先截取，再转换成 int ，再判断。我采用的做法是先转成 int，是合法的 ip 段数值以后，再截取。

3、由于 ip 段最多就 4 个段，因此这棵三叉树最多 4 层，这个条件作为递归终止条件之一；

4、每一个结点表示了求解这个问题的不同阶段，需要的状态变量有：

splitTimes：已经分割出多少个 ip 段；
begin：截取 ip 段的起始位置；
path：记录从根结点到叶子结点的一个路径（回溯算法常规变量，是一个栈）；
res：记录结果集的变量，常规变量。

作者：liweiwei1419
链接：https://leetcode.cn/problems/restore-ip-addresses/solution/hui-su-suan-fa-hua-tu-fen-xi-jian-zhi-tiao-jian-by/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
```java
class Solution {
    //画图理解
    public List<String> restoreIpAddresses(String s) {
        //定义表示一个字符长度的变量
        int len = s.length();
        //定义一个返回结果的集合
        List<String> res = new ArrayList<>();
        //如果当前字符长度大于12或者小于4都不满足
        if(len > 12 || len <4){
            return res;
        }
        //定义一个保存路径上的变量
        Deque<String> path = new ArrayDeque<>();
        //深度优先搜索
        dfs(s,len, 0, 4, path, res);
        //返回结果
        return res;
    }

    public void dfs(String s, int len, int begin, int residue, Deque<String> path, List<String> res){
        //如果字符串已经遍历到最后了，并且已经切分为4段了，
        //就把当前路径上的元素加入到返回的结果集中
        if(begin == len){
            if(residue ==0){
                res.add(String.join(".", path));
            }
            return;
        }
        //begin表示遍历字符串从哪里开始
        for(int i = begin; i < begin+3; i++){
            //如果超出字符串的长度，就直接退出
            //begin，每次选择都是从之前选择的元素的下一个元素开始，
            if(i >= len){
                break;
            }
            //如果剩余元素大于ip最多能容纳的个数，就剪枝。
            if(len -i > residue * 3){
                continue;
            }
            //判断当前截取字符是否是小于0或者大于255
            //这里的begin和i，代表的是，这时候截取了几个字符
            //begin从哪里开始，i到哪里结束
            if(judgeIpSegment(s, begin, i)){
                //保留当前截取字符
                String currentIpSegment = s.substring(begin, i+1);
                //将当前路径上的元素加入到路径队列中
                path.addLast(currentIpSegment);
                //递归下一层
                dfs(s, len, i+1,residue -1, path, res);
                //剪枝
                path.removeLast();
            }
        }
    }
    private boolean judgeIpSegment(String s, int left, int right){
        //定义一个表示整个字符的长度
        int len = right - left +1;
        //如果截取的大于等于2的字符的开头为0，就直接false
        if(len > 1 && s.charAt(left) == '0'){
            return false;
        }
        //定义返回结果的集合
        int res = 0;
        //拼接字符
        while(left <= right){
            //res*10 是为了将先加的字符默认比后面的字符大10倍，也就是位数是从小到大
            res = res * 10 + s.charAt(left) - '0';
            left++;
        }
        return res >= 0 && res <= 255;
    }
}

```
