## Tag
stack, 堆成匹配
## 审题（关键词） 
Duplicate removal, adjacent and equal(duplicate), 
## 初始思路  
暴力思路： 
使用hashmap，key 是 char, value 是 frequency, 但很难处理**相邻**这个条件。

注意到关键词相连的，所以我们需要反复比较当前元素和上一个入栈元素即相邻元素。
且移除相邻元素后又会出现新的匹配，比如 zxxz。

那么可以用栈，利用栈的LIFO。
和20括号匹配思路一致。

## 考点  
栈的对称匹配题型

## 解法  
```java
class Solution {
    public String removeDuplicates(String s) {
        // 创建一个字符栈
        Stack<Character> stack = new Stack<>();

        // 将输入字符串 s 转换为字符数组 chrs
        char[] chrs = s.toCharArray();
        // 或者使用 s.charAt(index);

        // 遍历字符数组 chrs 中的每一个字符 c
        for (char c : chrs) {
            // 如果栈为空，或者栈顶元素与字符 c 不相等
            if (stack.isEmpty() || stack.peek() != c) {
                // 将字符 c 压入栈中
                stack.push(c);
            } else {
                // 否则，弹出栈顶元素（即移除重复的相邻字符）
                stack.pop();
            }
        }

        // 使用 StringBuilder 优化字符串拼接
        StringBuilder ret = new StringBuilder();
        // 当栈不为空时，执行以下操作
        while (!stack.isEmpty()) {
        // 弹出栈顶元素，并将其添加到 StringBuilder ret 的末尾
        ret.append(stack.pop());
        }

        // 返回去重后的字符串
        return ret.toString();
    }
    
    public String removeDuplicates2(String s) {
        // 将 res 当做栈
        // 也可以用 StringBuilder 来修改字符串，速度更快
        // StringBuilder res = new StringBuilder();
        StringBuffer res = new StringBuffer();
        // top为 res 的长度
        int top = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            // 当 top > 0,即栈中有字符时，当前字符如果和栈中字符相等，弹出栈顶字符，同时 top--
            if (top >= 0 && res.charAt(top) == c) {
                res.deleteCharAt(top);
                top--;
            // 否则，将该字符 入栈，同时top++
            } else {
                res.append(c);
                top++;
            }
        }
        return res.toString();
    }
}
```

## 难点
如何想到使用栈。
