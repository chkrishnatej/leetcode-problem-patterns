# Searching BST

> n -> Number of nodes in tree

{% hint style="info" %}
```
Time complexity: 
* Best Case:  O(H), H-> Height of the tree
* Worse Case: O(n) (skewed BST)
* Average case: O(log n)
```
{% endhint %}

{% hint style="success" %}
```
Space complexity: 
* Best Case: O(H), H-> Height of the tree
* Worse Case: O(n) (skewed BST)
* Average case: O(log n)
```
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
    public TreeNode searchBST(TreeNode root, int target) {
        // Time complexity: O(H), H-> Height of the tree
        // Worse Case: O(n) (skewed BST), Average case: O(log n)
        // Space complexity: O(H), H-> Height of the tree
        // Worse Case: O(n) (skewed BST), Average case: O(log n)
         
        if(root==null) {
            return null;
        }
        
        if(target == root.val) {
            return root;
        } else if(target < root.val) {
            return searchBST(root.left, target);
        } else {
            return searchBST(root.right, target);
        }
    }
}
```
