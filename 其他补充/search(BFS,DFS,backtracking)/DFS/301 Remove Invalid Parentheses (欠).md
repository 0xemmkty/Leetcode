# 301. Remove Invalid Parentheses (欠)

Given a string s that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.

Return all the possible results. You may return the answer in any order.

## Solution 1
今天这道题，我们简单点的做法呢，就是先统计出来一共要删除多少个左括号和多少个右括号，然后，再递归遍历给定字符串中的字符，遇到左右括号的时候分两种情况，要 和 不要 ，当遍历完成的时候判断最终的字符串是不是合法的即可，代码比较简单。

作者：tong-zhu
链接：https://leetcode.cn/problems/remove-invalid-parentheses/solution/tong-ge-lai-shua-ti-la-yi-bu-bu-shen-du-jvrgz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        // 先统计需要删除的左括号数和右括号数
        int leftExtra = 0, rightExtra = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '(') {
                leftExtra++;
            } else if (c == ')') {
                if (leftExtra == 0) {
                    rightExtra++;
                } else {
                    leftExtra--;
                }
            }
        }

        // 如果字符串本身就是合法的，就不用删除了
        if (leftExtra == 0 && rightExtra == 0) {
            return Arrays.asList(s);
        }

        List<String> ans = new ArrayList<>();

        // 回溯
        dfs(ans, s, 0, 0, new StringBuilder(), leftExtra, rightExtra, 0);

        return ans;
    }

    private void dfs(List<String> ans, String s, int i, int diff, StringBuilder sb, int leftExtra, int rightExtra, int status) {
        if (i == s.length()) {
            if (diff == 0 && isValid(sb)) {
                ans.add(sb.toString());
            }
            return;
        }

        char c = s.charAt(i);
        if (c == '(') {
            // 最低位为1说明前面的左括号没要，那这个左括号也不能要
            if ((status & 1) == 0) {
                // 要这个左括号
                sb.append(c);
                dfs(ans, s, i + 1, diff + 1, sb, leftExtra, rightExtra, 0);
                sb.deleteCharAt(sb.length() - 1);
            }

            // 不要这个左括号
            if (leftExtra > 0) {
                // 不要当前这个左括号，后面的左括号也不能要
                dfs(ans, s, i + 1, diff, sb, leftExtra - 1, rightExtra, status | 1);
            }
        } else if (c == ')') {
            // 要这个右括号
            // 倒数第二位为1说明前面的右括号没要，那这个右括号也不能要
            if (diff > 0 && (status & 2) == 0) {
                sb.append(c);
                dfs(ans, s, i + 1, diff - 1, sb, leftExtra, rightExtra, 0);
                sb.deleteCharAt(sb.length() - 1);
            }

            // 不要这个右括号
            if (rightExtra > 0) {
                // 不要当前这个右括号，后面的右括号也不能要
                dfs(ans, s, i + 1, diff, sb, leftExtra, rightExtra - 1, status | 2);
            }
        } else {
            sb.append(c);
            dfs(ans, s, i + 1, diff, sb, leftExtra, rightExtra, 0);
            sb.deleteCharAt(sb.length() - 1);
        }

    }

    private boolean isValid(StringBuilder sb) {
        // 判断括号是否合法
        int left = 0;
        for (int i = 0; i < sb.length(); i++) {
            char c = sb.charAt(i);
            if (c == '(') {
                left++;
            } else if (c == ')') {
                if (left == 0) {
                    return false;
                } else {
                    left--;
                }
            }
        }
        return left == 0;
    }
}

作者：tong-zhu
链接：https://leetcode.cn/problems/remove-invalid-parentheses/solution/tong-ge-lai-shua-ti-la-yi-bu-bu-shen-du-jvrgz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```