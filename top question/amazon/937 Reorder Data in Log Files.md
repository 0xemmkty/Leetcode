# 937. Reorder Data in Log Files

You are given an array of logs. Each log is a space-delimited string of words, where the first word is the identifier.

There are two types of logs:

Letter-logs: All words (except the identifier) consist of lowercase English letters.
Digit-logs: All words (except the identifier) consist of digits.
Reorder these logs so that:

The letter-logs come before all digit-logs.
The letter-logs are sorted lexicographically by their contents. If their contents are the same, then sort them lexicographically by their identifiers.
The digit-logs maintain their relative ordering.
Return the final order of the logs.

数字日志和字母日志
数字日志按原有顺序，字母日志的顺序需要重新排（见python代码）
## s1（简单排序）
```java
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        List<String> l1 = new ArrayList<>();  // 存放字母日志
        List<String> l2 = new ArrayList<>();  // 存放数字日志

        for (String log : logs) {
            char c = log.charAt(log.length() - 1);
            boolean isNum = Character.isDigit(c);  // 判断每条log最后一个字符是否为数字
            if (!isNum) {
                l1.add(log);
            } else {
                l2.add(log);
            }
        }
        // 对字母日志排序： 
        Collections.sort(l1, (o1, o2) -> {
            String s1 = o1.substring(o1.indexOf(' '));
            String s2 = o2.substring(o2.indexOf(' '));
            if (s1.equals(s2)) {  // 如果内容相同时，直接比较原log字符串
                return o1.compareTo(o2);
            } else {  // 否则，按内容字母顺序排序
                return s1.compareTo(s2);
            }
        });
        l1.addAll(l2);  // 把数字日志的所有元素添加到l1
        return l1.toArray(new String[0]);
    }
}
```

```python
class Solution(object):
    def reorderLogFiles(self, logs):
        """
        :type logs: List[str]
        :rtype: List[str]
        """
        letters = []
        nums = []
        for log in logs:
            logsplit = log.split(" ")
            if logsplit[1].isalpha():
                letters.append((" ".join(logsplit[1:]), logsplit[0]))
            else:
                nums.append(log)
        letters.sort()
        return [letter[1] + " " + letter[0] for letter in letters] + nums
```