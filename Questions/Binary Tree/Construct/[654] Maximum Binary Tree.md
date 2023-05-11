> 看似简单但是可以很难的题，理解线段树，笛卡尔树，复习递归解法，栈解法以及线段树解法。甚至还有动态规划解法。
## Resource :
- [笛卡尔树 - OI Wiki](https://oi-wiki.org/ds/cartesian-tree/)
- [线段树 - OI Wiki](https://oi-wiki.org/ds/seg/)
---
其实本树就是一个笛卡尔树，唯一不同是笛卡尔树是最小值。

> 笛卡尔树是一种二叉树，它满足以下两个条件：

> 笛卡尔树中的每个节点都包含一个键值对 (key, value)。对于树中的任意节点，其左子树的所有节点的键都小于该节点的键，右子树的所有节点的键都大于该节点的键。这意味着笛卡尔树的中序遍历结果是关键字的有序序列。
> 
> 对于树中的任意节点，其 value 是其子树中所有节点的 value 的最小值。这个条件保证了笛卡尔树的结构是唯一的。
> 
> 其他概念：
> 
> 线段树是一棵完全二叉树，可以通过递归的方式构建。每个节点表示一个区间，左子节点表示区间的左半部分，右子节点表示区间的右半部分。节点中存储的是区间内的最大值及其对应的索引。
>
# [Maximum Binary Tree - LeetCode](https://leetcode.com/problems/maximum-binary-tree/description/)
## Tag
binary Tree, Construct, PreOrder, Segment Tree, Stack, Cartesian Tree

## 审题（关键词） 
构造最大二叉树，递归性

## 初始思路  
递归思路：  
前序遍历，  
需要参数：nums， 区间，返回值：TreeNode  
处理：找到当前区间的最大值，  
访问：左边和右边分别的区间  
循环不变量：左闭右开  

思考：如何快速判断每个区间的最大值？如果只用遍历的话，则是O(n^2)时间复杂度  
	线段树？  

## 考点  
构造二叉树，前序遍历  

### 二叉树构建技巧：
本质就是寻找分割点，分割点作为当前节点，然后递归左区间和右区间。
代码：使用前序遍历

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
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        // 递归思路：
        // 前序遍历，
        // 需要参数：nums， 区间，返回值：TreeNode
        // 预处理：找到当前区间的最大值，
        // 访问：左边和右边分别的区间
        // 处理：构建树
        // 循环不变量：左闭右开
        return construct(nums, 0, nums.length);
    }

    // 注意循环不变量：左闭右开
    private TreeNode construct(int[] nums, int left, int right) {
        if (left >= right) {
            return null;
        }

        TreeNode node = new TreeNode();
        
        // find the max index:
        int max = left;
        // 左闭右开
        for (int i = left + 1; i < right; i++) {
            max = nums[i] > nums[max] ? i : max;
        } 
        node.val = nums[max];

        // 左闭右开
        node.left = construct(nums, left, max);
        node.right = construct(nums, max + 1, right);
        return node;
    }
}
```

## 难点
如何让搜索区间最大值更快？
第一反应式线段树，
> 线段树是一棵完全二叉树，可以通过递归的方式构建。每个节点表示一个区间，左子节点表示区间的左半部分，右子节点表示区间的右半部分。节点中存储的是区间内的最大值及其对应的索引。
通过线段树的使用可以把时间复杂度降到O(nlogn)

但是了解到还有更好的**单调栈**的解法  
（一开始确实有想过这种最大值用单调栈，但是没想清楚pop元素时的操作）  
具体操作入下：
1. 保持单调递减栈
2. 元素入栈，如果没有破坏单调性（< 栈顶元素），
	1. 栈顶元素 是 新元素的 父元素（左边最大的值）
	2. 建立 栈顶.right = 新元素 的关系
3. 当出现破坏单调性的元素（新元素> 栈顶元素），也就意味着：
	1. 新元素 是 栈顶元素的 右边最大值，
	2. 建立 新元素.left = 栈顶元素 的关系

```java
import java.util.Stack;

public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        Stack<TreeNode> stack = new Stack<>();

        for (int num : nums) {
            TreeNode newNode = new TreeNode(num);

            // 当栈非空且栈顶元素小于当前元素时，持续弹出栈顶元素
            while (!stack.isEmpty() && stack.peek().val < newNode.val) {
                // 将弹出元素设置为新节点的左子节点
                newNode.left = stack.pop();
            }

            // 如果栈非空，说明栈顶元素是当前元素左侧第一个比它大的元素
            // 设置栈顶元素的右子节点为新节点
            if (!stack.isEmpty()) {
                stack.peek().right = newNode;
            }

            // 将新节点入栈
            stack.push(newNode);
        }

        // 最后，栈底元素即为整棵树的根节点
        TreeNode root = null;
        while (!stack.isEmpty()) {
            root = stack.pop();
        }

        return root;
    }
}
```


PS. 线段树解法：
```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class SegmentTree {
    private int[] data; // 存储原始数据的数组
    private int[] tree; // 存储线段树的数组

    // 构造函数
    public SegmentTree(int[] nums) {
        data = new int[nums.length];
        System.arraycopy(nums, 0, data, 0, nums.length);

        tree = new int[4 * nums.length];
        buildSegmentTree(0, 0, nums.length - 1);
    }

    // 构建线段树的递归函数
    private void buildSegmentTree(int treeIndex, int l, int r) {
        if (l == r) {
            tree[treeIndex] = l;
            return;
        }

        int leftTreeIndex = leftChild(treeIndex);
        int rightTreeIndex = rightChild(treeIndex);

        int mid = l + (r - l) / 2;
        buildSegmentTree(leftTreeIndex, l, mid);
        buildSegmentTree(rightTreeIndex, mid + 1, r);

        if (data[tree[leftTreeIndex]] > data[tree[rightTreeIndex]]) {
            tree[treeIndex] = tree[leftTreeIndex];
        } else {
            tree[treeIndex] = tree[rightTreeIndex];
        }
    }

    // 查询区间最大值索引的公共接口
    public int query(int queryL, int queryR) {
        return query(0, 0, data.length - 1, queryL, queryR);
    }

    // 查询区间最大值索引的递归函数
    private int query(int treeIndex, int l, int r, int queryL, int queryR) {
        if (l == queryL && r == queryR) {
            return tree[treeIndex];
        }

        int mid = l + (r - l) / 2;
        int leftTreeIndex = leftChild(treeIndex);
        int rightTreeIndex = rightChild(treeIndex);

        if (queryL >= mid + 1) {
            return query(rightTreeIndex, mid + 1, r, queryL, queryR);
        } else if (queryR <= mid) {
            return query(leftTreeIndex, l, mid, queryL, queryR);
        }

        int leftResult = query(leftTreeIndex, l, mid, queryL, mid);
        int rightResult = query(rightTreeIndex, mid + 1, r, mid + 1, queryR);

        if (data[leftResult] > data[rightResult]) {
            return leftResult;
        } else {
            return rightResult;
        }
    }

    // 获取左孩子节点的索引
    private int leftChild(int index) {
        return 2 * index + 1;
    }

    // 获取右孩子节点的索引
    private int rightChild(int index) {
        return 2 * index + 2;
    }
}

public class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        if (nums == null || nums.length == 0) {
            return null;
        }

        // 构建线段树
        SegmentTree segmentTree = new SegmentTree(nums);
        return buildMaxBinaryTree(nums, segmentTree, 0, nums.length - 1);
    }
```

动态规划法：
```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class CartesianTree {
    public TreeNode buildCartesianTree(int[] nums) {
        if (nums == null || nums.length == 0) {
            return null;
        }

        TreeNode[] dp = new TreeNode[nums.length];
        for (int i = 0; i < nums.length; i++) {
            TreeNode newNode = new TreeNode(nums[i]);
            TreeNode lastNodeOnRight = null;

            // 更新 dp 数组
            for (int j = i - 1; j >= 0; j--) {
                if (dp[j].val < newNode.val) {
                    if (lastNodeOnRight == null) {
                        newNode.left = dp[j].right;
                    } else {
                        lastNodeOnRight.left = dp[j].right;
                    }
                    dp[j].right = newNode;
                    dp[i] = dp[j];
                    break;
                }
                lastNodeOnRight = dp[j];
                if (j == 0) {
                    newNode.left = dp[0].right;
                    dp[i] = newNode;
                }
            }

            if (i == 0) {
                dp[i] = newNode;
            }
        }

        return dp[nums.length - 1];
    }

    public static void main(String[] args) {
        int[] nums = {9, 3, 7, 1, 8, 12, 10, 20, 15, 18, 5};
        CartesianTree ct = new CartesianTree();
        TreeNode root = ct.buildCartesianTree(nums);
    }
}

```