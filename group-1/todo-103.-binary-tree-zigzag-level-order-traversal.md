# TODO: 103. Binary Tree Zigzag Level Order Traversal

## Problem

{% embed url="https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/" %}

## Intuition



## Time Complexity

`O(V+E)` -> Standard BFS time complexity

## Space Complexity

`O(V)` -> Standard BFS space complexity

## Solution

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        // TC: O(V+E)
        List<List<Integer>> result = new ArrayList<>();

        if(root == null) {
            return result;
        }

        // Performing standard BFS
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);

        // Used as flag to understand if elements in level have to be ordered 
        // from left to right or vice-verca
        int level = 0;

        while(!q.isEmpty()) {
            int n = q.size();

            List<Integer> current = new LinkedList<>();

            for(int i=0;i<n;i++) {
                TreeNode node = q.poll();
                // This toggle controls, in which order the elements will be 
                // inserted to current
                if(level % 2 == 0) {
                    current.addLast(node.val);
                } else {
                    current.addFirst(node.val);
                }

                if(node.left != null) q.offer(node.left);
                if(node.right != null) q.offer(node.right);
            }
            result.add(current);
            level++;
        }
        return result;
    }
}
```
