# 71. Simplify Path
(压栈匹配)

Given a string path, which is an absolute path (starting with a slash '/') to a file or directory in a Unix-style file system, convert it to the simplified canonical path.

In a Unix-style file system, a period '.' refers to the current directory, a double period '..' refers to the directory up a level, and any multiple consecutive slashes (i.e. '//') are treated as a single slash '/'. For this problem, any other format of periods such as '...' are treated as file/directory names.

The canonical path should have the following format:

The path starts with a single slash '/'.
Any two directories are separated by a single slash '/'.
The path does not end with a trailing '/'.
The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period '.' or double period '..')
Return the simplified canonical path.

## Solution 1 (压栈匹配)
```
理解题意：给定一个路径path，进行简化，分为如下几种情况：

多个连续斜杠'///'简化为一个 '/'
一个点'.'表示当前目录，直接去除即可
两个点'..'表示上级目录，需返回到上一级
解题思路：

当遇到两个点时，需返回上级目录，类似于栈弹出栈顶元素的操作，因此想到使用栈！
为什么栈只存储文件名，不存储'/'？
存在多个连续斜杠的情况需要去除，且不便于返回上级目录(弹出栈顶元素)，故不存储！
实际操作：
用一个栈维护路径中的 文件/目录名

用vector模拟stack，最后便于返回目录
遍历路径path，分为两种情况：

遇到'/'，直接跳过，不做操作
遇到非斜杠，统计两斜杠间的字符串，再进行判断！
当为一个点'.'时，表示同级目录，不进行操作 跳过
当为两个点'..'表示上级目录，弹出栈顶元素，即返回上级目录
注意：可能当前在根目录(栈为空)，根目录没有上一级，则直接跳过即可
当为其余字符即文件名时，直接入栈

```
```java
class Solution {
    public String simplifyPath(String path) {
        Deque<String> stack = new LinkedList<>();
        for (String item : path.split("/")) {
            if (item.equals("..")) {
                if (!stack.isEmpty()) stack.pop();
            } else if (!item.isEmpty() && !item.equals(".")) stack.push(item);
        }
        String res = "";
        for (String d : stack) res = "/" + d + res;
        return res.isEmpty() ? "/" : res;  
    }
}
```