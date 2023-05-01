> 单调队列经典题，通过本题体会如何维护单调性。
## Resource
[代码随想录](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html)

# [Sliding Window Maximum - LeetCode](https://leetcode.com/problems/sliding-window-maximum/description/)
## Tag
sliding window, monotonic queue
## 审题（关键词）
sliding window, max number in sliding window

## 初始思路  
窗口滑动时，发生了什么：
1. 一个更大的值进入，更新当前最大值
2. 一个较小值进入，没有更新
3. 新值进入后，怎么判断之前的最大值没有在窗口最左边被移除？

滑动窗口有两个步骤：增加新值，减少旧值
与队列的一端进一端出呼应。

如果是只记录最大值则比较简单，但是窗口滑动后最大值会发生变化
1. 之前的最大值不变
2. 被窗口滑动移除，换次大值
3. 被新值顶替。

所以需要一个ds，保持单调性，记录最大，次大。。  
而且滑动窗口是FIFO，所以可以想到单调队列  

由于我们需要保留窗口最大值，所以保持单调队列单调性递减即可，队头的元素是最大的
### 思考
1. 为什么不使用优先级队列？  
   -  优先级队列不保存数组原本的顺序。窗口滑动时，排出的元素无法在优先级队列中找到。
2. 使用单调递增队列还是单调递减？  
   - 单调递减队列，这样保证最大值，次大值 在队列的头部，方便排出时进行比较
	
## 考点  
单调队列, 如何想到单调队列，如何维护单调性

## 解法  
Reference:
> 设计单调队列的时候，pop，和push操作要保持如下规则： 
> 
> 1. pop(value)：如果窗口移除的元素value等于单调队列的出口元素，那么队列弹出元素，否则不用任何操作
> 
> 2. push(value)：如果push的元素value大于入口元素的数值，那么就将队列入口的元素弹出，直到push元素的数值小于等于队列入口元素的数值为止
> 
> 保持如上规则，每次窗口移动的时候，只要问que.front()就可以返回当前窗口的最大值。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        // 双指针滑动窗口:
        int l = 0, r = 0;
        MyQueue myqueue = new MyQueue();
        List<Integer> ans = new ArrayList<>();
        
        // 移动k次，填满窗口
        while (r < k) {
                myqueue.add(nums[r]);
                r++;
        }
        // 加入第一个窗口的最大值。    
        ans.add(myqueue.getMax());

        while (r < nums.length) {
            // 移动窗口：
            // 排出窗口左边的元素：
            myqueue.poll(nums[l]);
            l++;
            // 加入窗口右边的元素：
            myqueue.add(nums[r]);
            r++;
            ans.add(myqueue.getMax());
        }

        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}

class MyQueue {
    // 双端单调递减队列
    private Deque<Integer> myqueue;

    public MyQueue() {
        myqueue = new LinkedList<>();
    }

    // poll 左边相等的元素
    // 滑动窗口排出元素时，比较该元素和当前最大元素，
    // 如果相等，说明该元素正在被排出，
    // 如果不等，说明这个元素前面的元素被排出
    public void poll(int a) {
        if (!myqueue.isEmpty() && myqueue.peek() == a) {
            myqueue.pollFirst();
        }
    }

    public Integer getMax() {
        return myqueue.peekFirst();
    }

    // 增加新元素，保持队列内部的单调性（递减）
    public void add(int a) {
        while (!myqueue.isEmpty() && myqueue.peekLast() < a) {
            myqueue.pollLast();
        }

        myqueue.add(a);
    }
}
```
