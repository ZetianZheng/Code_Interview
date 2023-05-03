## Resource:
[代码随想录 括号匹配](https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E8%BF%9B%E5%85%A5%E6%AD%A3%E9%A2%98)
# [Valid Parentheses - LeetCode](https://leetcode.com/problems/valid-parentheses/description/)
## Tag
Stack, 括号匹配
## 审题（关键词） 
String, isValid, Parentheses(Pair)

## 初始思路  
括号一般是**成对**出现的。  

判断valid也就是判断括号是否都有**成对**，且左右括号的位置**无法颠倒**。  

判断成对的方法：
1. 计算左右括号的数量
   1. 简化问题： 只有一个括号对()。
   2. 那么可以使用一个counter = 0来记录数量，**'(' +1, ')' -1**, **不能出现<0**（无法颠倒）最后看是否等于0（是否都有对）
   3. “[{]}” 这种情况不匹配。所以单纯计算数量不可取。
	
2. 使用栈
   1. 简化问题： 只有一个括号对()
   2. 使用一个栈左括号入栈，右括号出栈，最后看是否栈为空（无法颠倒且每一个括号都有对）
   3. 如果有多个括号，则还要加条件，出栈时需要判断当时括号是否和栈顶匹配。

## 考点  
栈的对称匹配运用（括号匹配）  
几种匹配不等的情况：

1. 第一种情况，字符串里左方向的括号多余了 ，所以不匹配。 括号匹配1 {{{}
2. 第二种情况，括号没有多余，但是 括号的类型没有匹配上。 括号匹配2 [{]}
3. 第三种情况，字符串里右方向的括号多余了，所以不匹配。{}}}
## 解法  
```java
// 定义一个名为 Solution 的类
class Solution {
    // 定义一个名为 isValid 的公共方法，接收一个字符串 s 作为参数，并返回一个布尔值
    public boolean isValid(String s) {
        // 创建一个字符栈
        Stack<Character> stack = new Stack<>();
        // 创建一个括号映射
        Map<Character, Character> parenthesesMap = createParentheseMap();
    
        // 将输入字符串 s 转换为字符数组 chrs
        char[] chrs = s.toCharArray();
        
        // 遍历字符数组 chrs 中的每一个字符 c
        for (char c : chrs) {
            // 如果字符 c 是一个左括号，将其压入栈中
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            }

            // 如果字符 c 是一个右括号
            if (c == ')' || c == '}' || c == ']') {

                // 如果栈为空，说明右括号没有与之匹配的左括号，返回 false
                if (stack.isEmpty()) {
                    return false;
                }

                // 从栈顶弹出一个字符，将其赋值给变量 top
                char top = stack.pop();
                // 获取字符 c 对应的左括号，并赋值给变量 current
                char current = parenthesesMap.get(c);

                // 如果栈顶字符 top 与 current 不匹配，返回 false
                if (top != current) {
                    return false;
                }
            }
        }

        // 如果栈的大小为 0，说明所有括号都已经匹配成功，返回 true，否则返回 false
        return stack.size() == 0;
    }

    // 定义一个名为 createParentheseMap 的私有方法，返回一个字符映射
    private Map<Character, Character> createParentheseMap() {
        // 创建一个名为 parenthesesMap 的哈希映射
        Map<Character, Character> parenthesesMap = new HashMap<>();
        // 将右括号 ')' 映射到左括号 '('
        parenthesesMap.put(')', '(');
        // 将右括号 '}' 映射到左括号 '{'
        parenthesesMap.put('}', '{');
        // 将右括号 ']' 映射到左括号 '['
        parenthesesMap.put(']', '[');
        // 返回括号映射
        return parenthesesMap;
    }
}
```

##  难点
我一开始以为 case2: [{]}是合法的，所以使用counter计数法以及想用三个栈分别记录不同的括号。
