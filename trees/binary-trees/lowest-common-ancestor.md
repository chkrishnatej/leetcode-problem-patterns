# Lowest Common Ancestor

## Problem

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/" %}

## Intution

Whenever we are given two nodes and asked to find the LCA, we recursively explore the left and right subtrees

When we reach leaf node or a node which is null, we will return `null`

But if we find a node or a value, we are looking for, we return that node. So whatever treenodes that are in the stack will be returning themselves&#x20;

We just traverse left and right subtree recursively

At the end there are three cases:

1. Left subtree is null and right subtree is null -> Then return null
2. Left Subtree is null -> Then return right subtree
3. Right Substree is null -> Then return left subtree

## Code

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Time Complexity: O(n), n -> number of nodes
        // Space Complexity: O(h), h -> Stack space
        
        // root == null -> Reached leaf nodes
        // root == p -> Found P node
        // root == q -> Found Q node
        // When we found what we want, we return the node itself as we do not 
        // want to explore further
        if(root == null) return null;
        if(root == p || root == q) return root;

        // Explore left and right recursively
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // If left node is given null, then pick right as we are not intersted in null values
        // Vice verca for right node
        // And for all other cases just return the current node
        if(left == null) return right; 
        else if(right == null) return left;
        else return root;
    }
}
```
