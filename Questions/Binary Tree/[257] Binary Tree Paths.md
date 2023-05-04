## Tag
PreOrder, BackTracking
## 审题（关键词） 
BInary Tree, Paths， 求所有路径
## 初始思路  
是递归思路，拆解问题， 本层的所有路径是下两个节点的所有路径加上本节点
什么时候处理？使用前序遍历，因为要让父节点指向子节点。
为什么要回溯？因为需要遍历所有的路径，在本路径访问完毕之后，需要回退到上某一步去试另一条支线。

## 考点  
PreOrder, BackTracking

## 解法  
```java
class Solution {
    List<List<Integer>> paths = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        // 遍历框架：前序遍历
        // 参数和定义：
        // 参数：root， List<Integer> 记录本路径所有节点
        // 定义，加入本节点，再往下遍历之后的子节点
        traverse(root, new ArrayList<Integer>());
        return transformer(paths);
    }

    private void traverse(TreeNode node, List<Integer> currPath) {
        // bc:
        if (node == null) {
            return; 
        }

        // 处理： 加入本节点：
        currPath.add(node.val);

        // 如果是叶子节点，更新本路径：
        if (node.left == null && node.right == null) {
            paths.add(new ArrayList<>(currPath));
        }
        
        // 访问
        traverse(node.left, currPath);
        traverse(node.right, currPath);

        // BackTracking： remove last element
        currPath.remove(currPath.size() - 1);
    }

    private List<String> transformer(List<List<Integer>> paths) {
        
        List<String> ans = new ArrayList<>();

        for (int i = 0; i < paths.size(); i++) {
            StringBuilder sb = new StringBuilder();

            List<Integer> path = paths.get(i);
            for (int j = 0; j < path.size(); j++) {
                sb.append(path.get(j));
                if (j != path.size() - 1) {
                    sb.append("->");        
                }
            }

            ans.add(sb.toString());
        }

        return ans;
    }
}
```
## 难点
注意是在叶子节点才更新答案，而不是空的时候更新答案。
