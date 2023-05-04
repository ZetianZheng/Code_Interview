# BFS_COMMON_QUESTIONS
- 102.二叉树的层序遍历
- 107.二叉树的层次遍历II
- 199.二叉树的右视图
- 637.二叉树的层平均值
- 429.N叉树的层序遍历
- 515.在每个树行中找最大值
- 116.填充每个节点的下一个右侧节点指针
- 117.填充每个节点的下一个右侧节点指针II
- 104.二叉树的最大深度
- 111.二叉树的最小深度

## [102 Binary Tree Level Order Traversal - LeetCode](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

> 经典层先遍历，[BFS.md](../../Template/BFS.md)
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        // use Queue
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        
        List<List<Integer>> ans = new ArrayList<>();

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            // iterate each level
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                // 访问：
                if (curr.left != null) queue.add(curr.left);
                if (curr.right != null) queue.add(curr.right);
                // 处理：
                level.add(curr.val);
            }

            ans.add(level);
        }

        return ans;
    }
}
```
## [107 Binary Tree Level Order Traversal II - LeetCode](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)
> 层先遍历，从叶子节点输出。just reverse the result of 102
```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        // use Queue
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        
        List<List<Integer>> ans = new ArrayList<>();

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            // iterate each level
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                // 访问：
                if (curr.left != null) queue.add(curr.left);
                if (curr.right != null) queue.add(curr.right);
                // 处理：
                level.add(curr.val);
            }

            ans.add(level);
        }
        
        Collections.reverse(ans);
        return ans;
    }
}
```

## [199 Binary Tree Right Side View - LeetCode](https://leetcode.com/problems/binary-tree-right-side-view/description/)
> 看树的右边。--> 仅记录每一层最后一个元素
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        // use Queue
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        
        List<Integer> ans = new ArrayList<>();

        while (!queue.isEmpty()) {
            int size = queue.size();
            // iterate each level
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                // 访问：
                if (curr.left != null) queue.add(curr.left);
                if (curr.right != null) queue.add(curr.right);
                // 处理：仅记录每一层最后一个元素
                if(i == size - 1) {
                    ans.add(curr.val);
                }
            }
        }
        
        return ans;
    }
}
```
## [637 Average of Levels in Binary Tree - LeetCode](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/)
> 每一层算平均值，注意double的赋值。
```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
         // use Queue
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        
        List<Double> ans = new ArrayList<>();

        while (!queue.isEmpty()) {
            int size = queue.size();
            Double sum = 0.0;
            // iterate each level
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                // 访问：
                if (curr.left != null) queue.add(curr.left);
                if (curr.right != null) queue.add(curr.right);
                // 处理：仅记录每一层的平均值
                sum += curr.val;
            }
            // 每一层遍历完毕后，记录平均值
            ans.add(sum/size);
        }
        
        return ans;
    }
}
```
## [429 N-ary Tree Level Order Traversal - LeetCode](https://leetcode.com/problems/n-ary-tree-level-order-traversal/description/)
> 多叉树的层先遍历，只需更改一下访问的部分即可（从左右子树，到多子树）
```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        // use Queue
        Queue<Node> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        
        List<List<Integer>> ans = new ArrayList<>();

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            // iterate each level
            for (int i = 0; i < size; i++) {
                Node curr = queue.poll();
                // access:
                for (int j = 0; j < curr.children.size(); j++) {
                    Node child = curr.children.get(j);
                    if (child != null) {
                        queue.add(child);
                    }
                }

                // process:
                level.add(curr.val);
            }

            ans.add(level);
        }

        return ans;
    }
}
```
## [515 Find Largest Value in Each Tree Row - LeetCode](https://leetcode.com/problems/find-largest-value-in-each-tree-row/description/)
> 求每层最大值
```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        // use Queue
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        
        List<Integer> ans = new ArrayList<>();

        while (!queue.isEmpty()) {
            int size = queue.size();
            int levelMax = Integer.MIN_VALUE;
            // iterate each level
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                // 访问：
                if (curr.left != null) queue.add(curr.left);
                if (curr.right != null) queue.add(curr.right);
                // 处理：
                levelMax = Math.max(levelMax, curr.val);
            }

            ans.add(levelMax);
        }

        return ans;
    }
}
```

## [116 Populating Next Right Pointers in Each Node - LeetCode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/description/)
> 链接每一层
```java
class Solution {
    public Node connect(Node root) {
        // use Queue
        Queue<Node> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }

        while (!queue.isEmpty()) {
            int size = queue.size();
            // set pre-node
            Node pre = queue.poll();
            if (pre.left != null) queue.add(pre.left);
            if (pre.right != null) queue.add(pre.right);

            // iterate each level
            // 注意： 第一个元素已经被取出了，这里从第二个元素开始
            for (int i = 1; i < size; i++) {
                Node curr = queue.poll();
                // 访问：
                if (curr.left != null) queue.add(curr.left);
                if (curr.right != null) queue.add(curr.right);
                // 处理：connect every nodes in this level
                pre.next = curr;
                pre = curr;
            }
        }

        return root;
    }
}
```
> 遍历思路（递归写法）[东哥带你刷二叉树（思路篇） :: labuladong的算法小抄](https://labuladong.github.io/algo/di-yi-zhan-da78c/shou-ba-sh-66994/dong-ge-da-cbce8/)
```java
class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        connectTwoNode(root.left, root.right);
        return root;
    }
    
    private void connectTwoNode(Node leftNode, Node rightNode) {
        // 定义：把下一层的所有node都链接起来。
        if (leftNode == null && rightNode == null) {
            return;
        }
        // 前序遍历，链接两个节点。
        leftNode.next = rightNode;
        
        connectTwoNode(leftNode.left, leftNode.right);
        connectTwoNode(rightNode.left, rightNode.right);
        connectTwoNode(leftNode.right, rightNode.left);
        
    }
}
```
## [117 Populating Next Right Pointers in Each Node II - LeetCode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/)
> code跟116 bfs解法完全一样

## [104 Maximum Depth of Binary Tree - LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
> iterate思维，BFS思路，每一层遍历之外level+1
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
> 递归思路，拆解问题，要本节点的最高节点：左右子节点高度的最大值+1
```java
class Solution {
    public int maxDepth(TreeNode root) {
        // base case:
        if (root == null) {
            return 0;
        }

        // traverse:
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);

        return Math.max(left, right) + 1;
    }
}
```
## [111 Minimum Depth of Binary Tree - LeetCode](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)
> 找最短路径经典题：BFS每一层遍历完才去找下一层，在找到目标的时候，depth一定是最小的
```java
class Solution {
    // 找最短路径
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int depth = 1;
        while (!queue.isEmpty()) {
            int size = queue.size();

            while (size > 0) {
                size--;
                TreeNode curr = queue.poll();
                if (curr.left == null && curr.right == null) {
                    // 找到最近的叶子节点
                    return depth;
                }
                if (curr.left != null)  
                    queue.add(curr.left);
                if (curr.right != null)
                    queue.add(curr.right);
            }

            depth++;
        }

        return depth;
    }
}
```
## [222 Count Complete Tree Nodes - LeetCode](https://leetcode.com/problems/count-complete-tree-nodes/description/)
> 不用记录层数，直接数节点数量
```java
class Solution {
    public int countNodes(TreeNode root) {
        // use Queue
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        
        int ans = 0;

        while (!queue.isEmpty()) {
            TreeNode curr = queue.poll();
            // 处理：
            ans++;
            // 访问：
            if (curr.left != null) queue.add(curr.left);
            if (curr.right != null) queue.add(curr.right);
        }

        return ans;
    }

}
```