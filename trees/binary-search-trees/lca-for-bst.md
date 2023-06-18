# LCA for BST

## Problem:

[https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

## Solution:

{% hint style="info" %}
### Time Complexity:

> n -> Number of nodes in BST

* Best case: `O(log n)`
* Worse case: `O(n)`, skewed BST
{% endhint %}

{% hint style="success" %}
### Space Complexity: O(1)
{% endhint %}

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
        /*
            Time Complexity: 
                * Best case: O(log n)
                * Worse case: O(n), skewed BST
            Space Complexity: O(1)
        */

        // terminating conditiion
        while(root!=null) {
            // If the p and q are smaller than root, search left
            if(p.val < root.val && q.val < root.val) {
                root = root.left;
            }
            // If the p and q are greater than root, search right
            else if(p.val > root.val && q.val > root.val) {
                root = root.right;
            } 
            // If that is not the case, we found the LCA, return the user
            else {
                return root;
            }
        }
        return null;
    }
}
```
