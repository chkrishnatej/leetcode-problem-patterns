# Sum of deepest leaves node

> n -> Number of nodes in Bina

{% hint style="info" %}
### Time Complexity:

`O(n)`

We need to traverse through all the nodes, no matter what, to figure out the deepest leaves.
{% endhint %}

{% hint style="success" %}
### Space Complexity:&#x20;

`O(1)`

No new space is allocated for calculating the deepest sum
{% endhint %}

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
    public int deepestLeavesSum(TreeNode root) {
        /*
            n -> Number of nodes in Binary Tree
            Time Complexity: O(n)
            Space Complexityin: O(1)
        */
        if(root == null) {
            return 0;
        }

        int sum = 0;
        TreeNode node = root;
        Deque<TreeNode> queue = new ArrayDeque<>();
        queue.add(node);

        int deepest = -1, current = 0;

        // Standard BFS traversal
        while(!queue.isEmpty()) {
            int size = queue.size();
            current++; // Keeping track of current level

            if(current > deepest) { // When new depth is found
                deepest = current; // Update depth as new found depth
                sum = 0; // Reset the sum to zero
            }
            // Here, we do not need to think about any multi-level, because we are 
            // tasked to find the deepest leaves sum. They will always be last,
            // Hence, we can simply return the sum
            for(int i=0;i<size;i++) {
                node = queue.poll();
               sum += node.val; // Summation of nodes at each level
                if(node.left != null) {
                    queue.add(node.left);
                }

                if(node.right != null) {
                    queue.add(node.right);
                }
            }
        }
        return sum;
    }
}
```
