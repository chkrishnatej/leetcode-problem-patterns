---
description: Level Order Traversal
---

# Breadth First Search (BFS)

> `n` --> Number of nodes

{% hint style="success" %}
* Time Complexity: `O(n)`
* &#x20;Space Complexity: `O(n)`
{% endhint %}

### Code

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
        // Stores the result
        List<List<Integer>> result = new ArrayList<>();
        // Edge caase
        if (root == null) {
            return result;
        }
        // Store the nodes to be processed
        Deque<TreeNode> deque = new LinkedList<>();
        deque.offer(root);
        

        while (!deque.isEmpty()) {
            // Get the number of elements to be processed at the current level
            int levelSize = deque.size();
            // Intermediate result for current level
            List<Integer> currentLevel = new ArrayList<>();
            // Iterate through deque
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = deque.poll();
                currentLevel.add(node.val);
                if (node.left != null) {
                    deque.offer(node.left);
                }
                if (node.right != null) {
                    deque.offer(node.right);
                }
            }
            result.add(currentLevel);
        }
        return result;
    }
}
```
