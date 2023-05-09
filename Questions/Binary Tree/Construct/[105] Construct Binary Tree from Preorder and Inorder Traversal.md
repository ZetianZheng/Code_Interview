# [Construct Binary Tree from Preorder and Inorder Traversal - LeetCode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
## Resource 
[代码随想录](https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF)
## Tag
Recursion, Binary Tree, preOrder, inOrder
## 审题（关键词） 
构造二叉树，前序，中序
## 初始思路  
前序或者后序确定根节点，中序确定左右子树，并递归生成。
## 考点  
preOrder, inOrder
## 解法  
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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // preorder 确定root的nodevalue，inorder确定左右子树
        // 我们可以很方便通过preorder的第一个元素得知root.val， 
        // 然后再inorder中找到这个val，左右子树的size也知道了，再通过preorder获得剩下两个子树的构造范围。
        // 核心还是二叉树的构造模板，构造一个二叉树，每层递归做的事情就是构造自己。
        if(preorder.length == 0) {
            return null;
        }
        return build(preorder, 0, preorder.length - 1,
                    inorder, 0, inorder.length - 1);
    }
    
    private TreeNode build(int[] preorder, int preLo, int preHi,
                          int[] inorder, int inLo, int inHi) {
        // BC：all nodes has been constructed
        if (preLo > preHi) {
            return null;
        }
        
        // 处理：
        // construct， 在前序中找到本根节点:
        int rootVal = preorder[preLo];
        TreeNode root = new TreeNode(rootVal);

        // find left tree and right tree，在中序中分割左右子树:
        int rootInd = -1;
        for (int i = inLo; i <= inHi; i++) {
            if (inorder[i]==rootVal) {
                rootInd = i;
                break;
            }
        }
        
        int leftSize = rootInd - inLo;

        // 访问：
        // recursion: 循环不变量：左闭右闭
        // build 左子树，在preorder中就是 preLo + 1 ~ preLo + leftSize, inorder 中就是左半边
        root.left = build(preorder, preLo + 1, preLo + leftSize,
                         inorder, inLo, inLo + rootInd - 1);

        // build 右子树，在preorder中就是 preLo + leftSize + 1 ~ preHi, inorder 中就是右半边
        root.right = build(preorder, preLo + leftSize + 1, preHi,
                          inorder, rootInd + 1, inHi);
        
        return root;
        
        
    }
}
```