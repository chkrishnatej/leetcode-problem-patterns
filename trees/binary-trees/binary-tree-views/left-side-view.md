# Left Side View

## Problem

{% embed url="https://leetcode.com/problems/binary-tree-right-side-view" %}

## Intution

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

If we carefully observe the above picture, we can understand that, the left view of binary tree is the left  most node or first node at every level

Perform regular BFS traversal and update the list only if the current element that is being processed is the first element in the level

## Time Complexity

`O(V+E)` -> Standard BFS time complexity

## Space Complexity

`O(1)`&#x20;

Auxillary  space complexity

## Code

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
    public List<Integer> rightSideView(TreeNode root) {
        // TC: O(V+E) 
        // SC: O(1)
        List<Integer> result = new ArrayList<>();

        if(root==null) {
            return result;
        }
        
        // Perform standard BFS
        Queue<TreeNode> q = new ArrayDeque<>();

        q.offer(root);

        while(!q.isEmpty()) {
            int n = q.size();

            for(int i=0;i<n;i++) {
                TreeNode node = q.poll();

                // TIP: Add the result only if it is right most in the current level
                if(i==0) result.add(node.val);
                
                if(node.left != null) q.offer(node.left);
                if(node.right != null) q.offer(node.right);
            }
        }
        return result;
    }
}
```

