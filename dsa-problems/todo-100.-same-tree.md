# TODO: 100. Same Tree

## Problem

[https://leetcode.com/problems/same-tree/description/](https://leetcode.com/problems/same-tree/description/)

## Intuition



## Time Complexity

`O(n)` -> Exploring all nodes

## Space Complexity

`O(h)` -> Stack space while recursion

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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // Both the nodes have got to terminal at same time
        if(p == null && q == null) { 
            return true;
        }

        // Either one of the node got to terminal -> not same trees
        if(p == null || q == null) {
            return false;
        }

        // Values differ -> not same trees
        if(p.val != q.val) {
            return false;
        }

        // Traversing the subtrees both left and right
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```
