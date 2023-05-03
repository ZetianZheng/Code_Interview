# [Maximum Depth of N-ary Tree - LeetCode](https://leetcode.com/problems/maximum-depth-of-n-ary-tree/description/) 
## Tag
PreOrder, PostOrder, BFS, BinaryTree, iteration, recursion
## 审题（关键词） 
N-ary Tree, maximum depth
## 初始思路  
同104题一致， 仅仅是访问形式的不同。
两个考点：  
详见： [104 Maximum Depth of Binary Tree](./%5B104%5D%20Maximum%20Depth%20of%20Binary%20Tree.md)
1. 遍历思路：求深度
2. 递归思路：求高度  

## 考点  
学习二叉树的两种解题思路, 分清树的高度和深度的不同。

## 解法  
 > 遍历思路，一层层往下遍历，（前序遍历求深度）用depth维护高度，res维护答案。注意要回溯高度
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    /**
    遍历思路， 求深度，使用前序遍历
     */
    // global value to record the maximum depth
    int max = 0;

    // Do we need extra params? Yes, need to record depth of each node
    public int maxDepth(Node root) {
        traverse(root, 0);
        return max;
    }

    void traverse(Node node, int depth) {
        if (node == null) {
            return;
        }
        // 处理：高度加1，比较最大值
        depth++;
        max = Math.max(depth, max);

        // 访问：处理每一个子节点。
        for (int i = 0; i < node.children.size(); i++) {
            traverse(node.children.get(i), depth);
        }

        // 后序位置回溯，维护高度。
        depth--;
    }
}
```
> 递归思路，问题拆解思路：先获知所有子节点深度，再跟新自己（后序遍历求高度）
```java

class Solution {
    /**
    递归思路：求高度，使用后序遍历
     */
    // global value to record the maximum depth
    int max = 0;

    // Do we need extra params? No, will update and process in the postOrder position, return the value
    public int maxDepth(Node root) {
        // bc: 
        if (root == null) {
            return 0;
        }
        int subMax = 0;

        // recursion: 
        for (int i = 0; i < root.children.size(); i++) {
            subMax = Math.max(subMax, maxDepth(root.children.get(i)));
        }

        // 处理：return the height of current node
        return subMax + 1;
    }
}
```

## 难点
