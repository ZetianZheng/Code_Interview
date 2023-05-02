# [Binary Tree Postorder Traversal - LeetCode](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)
## Tag
Binary Tree, PostOrder, Traversal
## 审题（关键词） 
Binary Tree, PostOrder, Traversal
## 初始思路  
1. 递归思路：
	1. 确定参数和返回值
	2. 确定返回值
	3. 确定单层的逻辑
2. 迭代思路：
	1. 先序遍历是中左右，后续遍历是左右中，
	2. 那么我们只需要调整一下先序遍历的代码顺序，就变成中右左的遍历顺序，
	3. 然后在反转result数组，输出的结果顺序就是左右中了

## 考点  
二叉树的后序遍历  
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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> postorder = new ArrayList<>();
        helper(root, postorder);
        
        return postorder;
    }
    
    public void helper(TreeNode root, List<Integer> postorder) {
        if (root == null) {
            return;
        }
        
        helper(root.left, postorder);
        helper(root.right, postorder);
        postorder.add(root.val);
    }
}
```
2. 迭代解法
先序遍历是中左右，后续遍历是左右中，  
那么我们只需要调整一下先序遍历调整成中右左的遍历顺序  
然后在反转result数组，输出的结果顺序就是左右中了  

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> postorder = new ArrayList<>();
        // 后序遍历： 左右中
        // 先先序遍历，中左右，调转顺序： 中右左
        Stack<TreeNode> stack = new Stack<>();
        if (root != null) {
            stack.push(root);
        }

        // 中右左
        while (!stack.isEmpty()) {
            TreeNode top = stack.pop();
            // 放入top的值
            postorder.add(top.val);

            // 先放入左，左后出
            if (top.left != null) {
                stack.push(top.left);
            }

            // 放入右，右先出
            if (top.right != null) {
                stack.push(top.right);
            }   
        }
        // 左右中
        Collections.reverse(postorder);

        return postorder;
    }
}
```
## 难点
