# [Path Sum - LeetCode](https://leetcode.com/problems/path-sum/description/)
## Tag
Recursion，backTracking
## 审题（关键词） 
求路径和
## 考点  
回溯，递归，穷举法

## 解法
> 递归解法，穷举所有的路径计算和。当是叶子节点的时候判断sum返回。注意回溯
```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        // 递归的第二个特性： 遍历所有路径，穷举
        if (root == null) {
            return false;
        }
        if (root.left == null && root.right == null) {
            return targetSum - root.val == 0;
        }

        boolean l = hasPathSum(root.left, targetSum - root.val);
        boolean r = hasPathSum(root.right, targetSum - root.val);
        return l || r;
    }
}
```
> 扩展： 迭代解法，两个栈分别记录节点和sum，同时处理
```java
class solution {
    public boolean haspathsum(treenode root, int targetsum) {
        if(root == null) return false;
        stack<treenode> stack1 = new stack<>();
        stack<integer> stack2 = new stack<>();
        stack1.push(root);
        stack2.push(root.val);
        while(!stack1.isempty()) {
            int size = stack1.size();

            for(int i = 0; i < size; i++) {
                treenode node = stack1.pop();
                int sum = stack2.pop();

                // 如果该节点是叶子节点了，同时该节点的路径数值等于sum，那么就返回true
                if(node.left == null && node.right == null && sum == targetsum) {
                    return true;
                }
                // 右节点，压进去一个节点的时候，将该节点的路径数值也记录下来
                if(node.right != null){
                    stack1.push(node.right);
                    stack2.push(sum + node.right.val);
                }
                // 左节点，压进去一个节点的时候，将该节点的路径数值也记录下来
                if(node.left != null) {
                    stack1.push(node.left);
                    stack2.push(sum + node.left.val);
                }
            }
        }
        return false;
    }
}
```

## 难点
