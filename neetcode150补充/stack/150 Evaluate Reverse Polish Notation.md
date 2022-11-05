# 150. Evaluate Reverse Polish Notation

>Evaluate the value of an arithmetic expression in **Reverse Polish Notation**.

>Valid operators are +, -, *, and /. Each operand may be an integer or another expression.

>Note that division between two integers should truncate toward zero.

>It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

```
逆波兰式（Reverse Polish Notation，RPN，或逆波兰记法），也叫后缀表达式（将运算符写在操作数之后）。
```
```
如：我们平时写a+b，这是中缀表达式，写成后缀表达式就是：ab+
(a+b)*c-(a+b)/e的后缀表达式为：
(a+b)*c-(a+b)/e
→((a+b)*c)((a+b)/e)-
→((a+b)c*)((a+b)e/)-
→(ab+c*)(ab+e/)-
→ab+c*ab+e/-
```
```
逆波兰表达式，也叫做后缀表达式。

我们平时见到的运算表达式是中缀表达式，即 "操作数① 运算符② 操作数③" 的顺序，运算符在两个操作数中间。
但是后缀表达式是 "操作数① 操作数③ 运算符②" 的顺序，运算符在两个操作数之后。

各种表达式没有本质区别，他们其实是同一个语法树，只是遍历方式不同而得到的不同式子；是一个事物的一体多面，只不过是从不同角度观察罢了。

中缀表达式是其对应的语法树的中序遍历；
后缀表达式是其对应的语法树的后序遍历；
前缀表达式是其对应的语法树的前序遍历；
```
## Solution 1 (STACK)
```
对逆波兰表达式求值的过程是：

如果遇到数字就进栈；
如果遇到操作符，就从栈顶弹出两个数字分别为 num2（栈顶）、num1（栈中的第二个元素）；计算 num1 运算 num2 .
逆波兰表达式是的代码实现很方便，用一个栈就能解决。
```
```JAVA
import java.util.Stack;

public class Solution {
    public int evalRPN(String[] tokens) {
        int a,b;
		Stack<Integer> S = new Stack<Integer>();
		for (String s : tokens) {
			if(s.equals("+")) {
				S.add(S.pop()+S.pop());
			}
			else if(s.equals("/")) {
				b = S.pop();
				a = S.pop();
				S.add(a / b);
			}
			else if(s.equals("*")) {
				S.add(S.pop() * S.pop());
			}
			else if(s.equals("-")) {
				b = S.pop();
				a = S.pop();
				S.add(a - b);
			}
			else {
				S.add(Integer.parseInt(s));
			}
		}	
		return S.pop();
	}
}
```


