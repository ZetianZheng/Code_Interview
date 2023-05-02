# [Binary Tree Preorder Traversal - LeetCode](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

## Tag
Binary Tree, Pre Order, Traversal

## 审题（关键词） 
Binary Tree, Pre Order, Traversal

## 初始思路  
1. 递归思路：
	1. 确定参数和返回值
	2. 确定返回值
	3. 确定单层的逻辑
2. 迭代思路：
	1. 不能
## 考点  
二叉树的前序遍历  
递归  
stack  

## 解法  
1. 递归解法
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> preOrder = new ArrayList<>();
        helper(preOrder, root);
        return preOrder;
    }

    private void helper(List<Integer> preOrder, TreeNode root) {
        if (root == null) {
            return;
        }

        preOrder.add(root.val);
        helper(preOrder, root.left);
        helper(preOrder, root.right);
    }
}
```
2. 迭代解法
利用栈的LIFO，右节点先进入，左节点后进入，这样从栈中取值的顺序就是：中，左， 右

```java
public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> preOrder = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();

        if (root != null) {
            stack.push(root);
        }
            
        // preorder: mid, left, right
        while (!stack.isEmpty()) {
            // 拿出中
            TreeNode top = stack.pop();

            preOrder.add(top.val);
            
            
            // LIFO, 右先入，左最后入
            // 放入右
            if(top.right != null) {
                stack.push(top.right);
            }
            
            // 放入左
            if (top.left != null) {
                stack.push(top.left);
            }
        }

        return preOrder;
    }
```
## 难点
