
> 两道练习queue和stack基本操作 的题目, 值得深入思考栈和队列的关系。
## Resource:
1. [队列实现栈以及栈实现队列 :: labuladong的算法小抄](https://labuladong.github.io/algo/di-yi-zhan-da78c/shou-ba-sh-daeca/dui-lie-sh-88541/)
2. [代码随想录 栈 队列](https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html)


# [Implement Queue using Stacks - LeetCode](https://leetcode.com/problems/implement-queue-using-stacks/description/)

## Tag:
stack, queue, FIFO, LIFO, 负负得正，线性表

## 审题（关键词） 
stack queue

## 初始思路
stack和queue的特征：
stack: 线性表：表尾插入和删除
queue: 线性表：一端插入，一端删除
要用stack 模拟queue的话，
可以用两个stack 模拟queue， 一个模拟表头，一个模拟表尾

## 考点  
栈和队列的基本知识点。FIFO and LIFO

## 解法  
比较有意思的是使用了负负得正的逆向思维，
同字符串翻转 [Reverse Words in a String - LeetCode](https://leetcode.com/problems/reverse-words-in-a-string/) 有异曲同工之妙。
具体解法： 
```
用栈实现队列
	两个栈，各管理队列S2头和S1尾，
	push 入队
		S1 push
	peek 看队头
		S1 倒入 S2(S2为空时）, 看S2 peek
	pop 队头
		S2 pop()
	
```

```java
/**
 * Java实现一个基于两个栈的队列
 */
class MyQueue {
    // 定义两个栈s_head和s_tail
    private Stack<Integer> s_head;
    private Stack<Integer> s_tail;

    // 初始化两个栈
    public MyQueue() {
        s_head = new Stack<>();
        s_tail = new Stack<>();
    }
    
    // 入队操作：将元素x压入s_tail栈
    public void push(int x) {
        s_tail.push(x);
    }
    
    // 出队操作：首先调用peek()方法确保s_head栈有元素，然后弹出s_head栈顶元素
    public int pop() {
        this.peek();
        return s_head.pop();
    }
    
    // 查看队首元素：确保s_head栈有元素，然后返回s_head栈顶元素
    public int peek() {
        if (s_head.isEmpty()) {
            // 若s_head栈为空，将s_tail栈中的元素依次弹出并压入s_head栈
            while (!s_tail.isEmpty()) {
                s_head.push(s_tail.pop());
            }
        }

        return s_head.peek();
    }
    
    // 判断队列是否为空：当两个栈都为空时，队列为空
    public boolean empty() {
        return s_head.isEmpty() && s_tail.isEmpty();
    }
}

```

# [Implement Stack using Queues - LeetCode](https://leetcode.com/problems/implement-stack-using-queues/description/)

## Tag
stack, queue, FIFO, LIFO，线性表
## 审题（关键词） 
stack queue
## 初始思路  
stack和queue的特征：
stack: 线性表：表尾插入和删除
queue: 线性表：一端插入，一端删除
要用queue 模拟stack的话，
用一个队列自己倒腾 + top element记录栈顶元素 即可实现。
## 考点  
栈和队列的基本知识点。
## 解法  
```
用队列实现栈
	一个队列 + top element 实现
	Push 入栈
		push队列, update top
	top看栈顶元素
		Top element
	pop栈顶
		队列前面的元素从头出来再从尾放进去
遇到数字进栈，遇到符号就栈顶两个元素出栈并运算，结果再进栈
```

```java
class MyStack {
    private Queue<Integer> queue;
    private Integer top_elem;

    public MyStack() {
        queue = new LinkedList<>();
        top_elem = 0;
    }
    
    public void push(int x) {
        queue.add(x);
        // 更新栈顶元素
        top_elem = x;
    }
    
    public int pop() {
        // 需要pop最后一个入队列的元素：
        int size = queue.size();
        while (size > 2) {
            queue.offer(queue.poll()); // 开头出的元素再从队尾塞回去
            size--;
        }
        
        // 倒2的元素升格为top element
        top_elem = queue.peek();
        queue.offer(queue.poll());
        // 输出最后一个栈顶元素（第一个入队列的元素）
        return queue.poll();
    }
    
    public int top() {
        // 为什么要记录top_elem, 不需要再折腾了。
        return top_elem;
    }
    
    public boolean empty() {
        return queue.isEmpty();
    }
}
```