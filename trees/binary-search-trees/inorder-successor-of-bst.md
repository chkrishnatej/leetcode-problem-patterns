# Inorder Successor of BST

## Problem

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

## Concept

We can find the next inorder successor by performing an inorder traversal. But that is not an efficient way. Why? Because we are not using any properties of the BST to find out what is the next inorder successor.

We need to understand where the successor could appear if you want to use BST properties.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Using BST properties, if we want to find the inorder successor

## Algorithm

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://leetcode.com/problems/inorder-successor-in-bst/Figures/285/img11.png" alt=""><figcaption></figcaption></figure>

## Solution

{% hint style="info" %}
### Time Complexity:

> n -> Number of nodes in BST

* Best Case: `O(log n)`&#x20;
* Worse Case: `O(n)`, skewed BST
{% endhint %}

{% hint style="success" %}
### Space Complexity: \`O(1)\`
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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        /*
        n -> Number of nodes in BST
        Time Complexity:
            * Best Case: O(log n)
            * Worse Case: O(n), skewed BST
        Space Complexity: O(1)
        */
        TreeNode successor = null;

        while(root!=null) {
            // If search value is greater than or equal to root value
            // The possible successor could be on right subtree
            // So traverse to right sub tree
            if(p.val >= root.val) {
                root = root.right;
            } 
            // If search value is lesser than root value
            // then successor must lie on left sub tree and the current 
            // node could also be a possible inorder successor
            else if(p.val < root.val){
                successor = root;
                root = root.left;
            }
        } 
        return successor;  
    }
}
```
