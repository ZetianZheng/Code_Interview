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