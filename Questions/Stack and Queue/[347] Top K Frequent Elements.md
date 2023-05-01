## Resource: 
- [二叉堆详解实现优先级队列 :: labuladong的算法小抄](https://labuladong.github.io/algo/di-yi-zhan-da78c/shou-ba-sh-daeca/er-cha-dui-1a386/)
- [代码随想录](https://programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html)
- [1.7 堆排序 | 菜鸟教程](https://www.runoob.com/w3cnote/heap-sort.html)
# [Top K Frequent Elements - LeetCode](https://leetcode.com/problems/top-k-frequent-elements/description/)
## Tag
min heap, priority queue
## 审题（关键词）
array, most frequent, k most

## 初始思路  
求前k个频率最高的数，
- 首先从频率关键词想到 如何统计频率：
		- 一般使用遍历一遍，使用map的形式统计频率
- 如何获取最高k个？
	- 第一想法是排序，取前k个，这样的话快排，O(nlogn)
	- 还有就是可以用一个数据结构：优先级队列，大顶堆实现类似的功能
	- 维护一个size为k的小顶堆，遍历完数组，保留在大顶堆里的就是答案。O(nlogk)
### 思考：
- 使用大顶堆还是小顶堆？
	- 小顶堆，这个思考需要前置思考：
		- 理解堆如何pop和push元素
		- 为什么称堆为优先级**队列**，堆在数组上如何实现
		- > 所以我们要用小顶堆，因为要统计最大前k个元素，只有小顶堆每次将最小的元素弹出，最后小顶堆里积累的才是前k个最大元素。
	
## 考点  
堆，优先级队列
## 解法  
``` java
class Solution {`
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();
        for (int n : nums) {
            freq.put(n, freq.getOrDefault(n, 0) + 1);
        }

        // 优先级队列 -> 小顶堆
        // 在新元素进来时较小的元素会不断被pop
        // 留下的就都是最大的元素。
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b)-> freq.get(a) - freq.get(b));

        // 遍历所有unique element
        for (int n : freq.keySet()) {
            pq.add(n);
            if (pq.size() > k) {
                pq.poll(); // delete the least frequent number.
            }
        }

        // convert to answer
        int[] ans = new int[k];
        for (int i = 0; i < k; i++) {
            ans[i] = pq.poll();
        }

        return ans;
    }
}
```
## 难点
想到堆，大顶堆和小顶堆的抉择
