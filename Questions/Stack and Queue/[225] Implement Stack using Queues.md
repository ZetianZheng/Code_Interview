# [Implement Stack using Queues - LeetCode](https://leetcode.com/problems/implement-stack-using-queues/description/)

## Tag
stack, queue, FIFO, LIFO，线性表
## 审题（关键词） 
stack queue
## 初始思路  
1. stack和queue的特征：  
   1. stack: 线性表：表尾插入和删除  
   2. queue: 线性表：一端插入，一端删除  
2. 要用queue 模拟stack的话，  
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