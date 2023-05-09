## Resource
[栈的最后表演！ | LeetCode：150. 逆波兰表达式求值_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1kd4y1o7on/?vd_source=11fa18bc276af3bba75dd7f376bfe9c9)
# [Evaluate Reverse Polish Notation - LeetCode](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

## Tag
Stack, Reverse Polish Notation

## 审题（关键词） 
Reverse Polish Notation

## 初始思路  
遇到数字进栈，遇到符号就栈顶两个元素出栈并运算，结果再进栈。  

本质还是一个消除操作： 栈顶两个数字遇到操作符进行一个消除操作。 

和20括号匹配（消除栈顶括号一致的）以及1047移除重复（消除栈顶字符一致）思路一致，只不过变换了一下触发消除的条件。  

## 考点 
栈的使用，后缀表达式（逆波兰表达式）。

## 解法  
```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        for (String s : tokens) {
            if ("+-*/".contains(s)) {
                stack.push(operate(stack.pop(), stack.pop(), s));
            } else {
                stack.push(Integer.parseInt(s));
            }
        }

        return stack.pop();
    }

    private int operate(int a, int b, String op) {
        int ans = 0;

        switch(op) {
            case "+":
                ans = a + b;
                break;
            case "-":
                ans = b - a;
                break;
            case "*":
                ans = a * b;
                break;
            case "/":
                ans = b / a;
                break;
        }

        return ans;
    }
}
```

## 难点
理解逆波兰表达式，如何想到栈。 
 
以中缀表达式 "(1 + 2) * (4 - 3)" 为例，我们可以用二叉树表示这个表达式。
   1. 二叉树的每个节点表示一个操作数或一个运算符。对于运算符节点，左子节点和右子节点分别表示运算符左侧和右侧的操作数或子表达式。

```
        *
      /   \
     +     -
    / \   / \
   1   2 4   3
```
这是一个表示中缀表达式 "(1 + 2) * (4 - 3)" 的二叉树。  

从树中可以很容易地看出操作数和运算符之间的关系以及运算的优先级。  

为了计算这个表达式，

我们可以采用后序遍历（左子树，右子树，根节点）的方式，这将产生与逆波兰表达式相对应的遍历结果：1 2 + 4 3 - *。

这种表示法允许我们在计算表达式时更直观地理解优先级和操作数与运算符之间的关系。