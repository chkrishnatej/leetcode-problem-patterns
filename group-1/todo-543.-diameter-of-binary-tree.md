---
description: '#binary-trees'
---

# TODO: 543. Diameter of Binary Tree

## Problem

{% embed url="https://leetcode.com/problems/diameter-of-binary-tree/description/" %}

## Intuition



## Time Complexity

`O(N)`  N-> number of nides

## Space Complexity

`O(1)` Auxillary space complexity

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
    int max = Integer.MIN_VALUE;

    public int diameterOfBinaryTree(TreeNode root) {
        // TC: O(N)
        getHeight(root);
        return max;
    }
    
    // This function while trying to find the height of the root tree, calculates the height of 
    // every subtree. To find the diameter, we have to explore all the nodes of the tree
    private int getHeight(TreeNode node) {
        if(node == null) return 0;

        int leftHt = getHeight(node.left);
        int rightHt = getHeight(node.right);

        // When we found the heights of the left subtree and right subtree, we check the 
        // diameter of it by adding them => leftHt + rightHt
        // Using a max tracker, we keep updating it every node
        max = Math.max(max, leftHt + rightHt);

        // Calculates the height of the tree
        return 1 + Math.max(leftHt, rightHt);
    }
}
```
