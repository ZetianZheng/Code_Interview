# [Maximum Depth of Binary Tree - LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
## Tag
PreOrder, PostOrder, BFS, BinaryTree, iteration, recursion
## 审题（关键词） 
Binary Tree, maximum depth

## 初始思路  
遍历二叉树，且返回最大路径，典型二叉树遍历题（不需要访问）
套用二叉树遍历框架：
1. 后序遍历解法：
	1. 递归思路
	2. 求高度（任意节点到叶子节点的距离）采用从下往上的计数，后序遍历
	4. 什么时候返回，要知道depth，它的子树都得访问，
	5. 递归
	6. 后序位置：计算最大值
	7. 需要额外的参数吗？不需要，使用递归的时候没有额外的对子问题的影响。
2. 前序遍历解法：
	1. 遍历思路
	2. 求深度（任意节点到根节点的距离）采用从上往下的计数遍历， 前序遍历
	3. 前序位置： 每个节点计算当前深度，需要记录这个高度并给子节点，且需要回溯维护。
	4. 前序位置：当前节点更新最大值。
	5. 子节点计算深度

扩展使用bfs解法。


## 考点  
学习二叉树的两种解题思路, 分清树的高度和深度的不同。

## 解法  
> 遍历思路，一层层往下遍历，（前序遍历求深度）用depth维护高度，res维护答案。注意要回溯高度
```java
class Solution {
    /**
      * 递归法(求深度法)
    */
    //定义最大深度, 使用全局变量记录答案。
    int maxnum = 0;

    public int maxDepth(TreeNode root) {
        // 前序遍历解法, 所有节点到根节点的距离，需要额外参数记录当前距离
        ans(root,0);
        return maxnum;
    }
    
    //递归求解最大深度
    void ans(TreeNode tr,int tmp){
        // base case
        if(tr==null) return;
        // 处理：前序位置
        tmp++;
        maxnum = maxnum<tmp?tmp:maxnum;
        // 访问
        ans(tr.left,tmp);
        ans(tr.right,tmp);
        // 回溯：
        tmp--;
    }
      
}
```
> 递归思路，问题拆解思路：先获知左右的节点深度，再跟新自己（后序遍历求高度）

```java
class Solution {
    public int maxDepth(TreeNode root) {
        // base case:
        if (root == null) {
            return 0;
        }

        // 访问
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        // 处理：后序位置
        return Math.max(left, right) + 1;
    }
}
```
> BFS, 一层层遍历，记录层数。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        // BFS Solution: get the depth
        // use Queue
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        int level = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();  
            // iterate each level
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                // 访问：
                if (curr.left != null) queue.add(curr.left);
                if (curr.right != null) queue.add(curr.right);
            }
            // 处理：
            level++;

        }

        return level;

    }
}
```

