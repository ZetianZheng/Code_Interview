# [Path Sum II - LeetCode](https://leetcode.com/problems/path-sum-ii/description/)
## Tag:
Recursion，backTracking
## 解法：
> 回溯理解
> 详见 [112 path sum](./%5B112%5D%20Path%20Sum.md)

```java
class Solution {
    List<List<Integer>> allPath = new ArrayList<>();

    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        // 递归
        // 参数：root, targetsum, current path
        // 单层递归逻辑：判断root 是否是叶子节点且targetSum = 0, 是的话将路径加入答案。
        pathSum(root, targetSum, new ArrayList<>());
        return allPath;
    }

    public void pathSum(TreeNode root, int targetSum, List<Integer> path) {
        if (root == null) {
            return;
        }

        // 处理： 前序判断是否需要更新答案：叶子节点且路径和为target
        path.add(root.val);

        if (root.left == null && root.right == null) {
            if(targetSum - root.val == 0) {
                allPath.add(new ArrayList<>(path));
            }
        }

        // 访问：注意隐藏回溯
        pathSum(root.left, targetSum - root.val, path);
        pathSum(root.right, targetSum - root.val, path);

        // backtracking
        path.remove(path.size() - 1);
    }
}
```