```java
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
                // access:
                if (curr.left != null) {
                    queue.add(curr.left);
                }
                
                if (curr.right != null) {
                    queue.add(curr.right);
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