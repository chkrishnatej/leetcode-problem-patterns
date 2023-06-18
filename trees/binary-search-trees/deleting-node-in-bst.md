# Deleting Node in BST

> ```
> n -> Number of nodes
> ```

{% hint style="info" %}
### Time Complexity:

* Worst case: `O(n)`&#x20;
* Best case (when the node to be deleted is a leaf): `O(log n)`&#x20;
* Average case: `O(log n)`
{% endhint %}

{% hint style="success" %}
### Space Complexity

* Worst case: `O(n)`
* Best case (when the node to be deleted is a leaf): `O(1)`&#x20;
* Average case: `O(log n)`
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
    public TreeNode deleteNode(TreeNode root, int key) {
        /*
        u
        Time complexity:
            * Worst case: O(n)
            * Best case (when the node to be deleted is a leaf): O(log n)
            * Average case: O(log n)
        Space complexity:
            * Worst case: O(n)
            * Best case (when the node to be deleted is a leaf): O(1)
            * Average case: O(log n)
        */

        // Base case
        if(root==null) {
            return null;
        }

        if(key < root.val) { // If target < root.val
            root.left = deleteNode(root.left, key); // search left
        } 
        else if(root.val < key) { // If target > root.val
            root.right = deleteNode(root.right, key); // search right
        } 
        else { // If target is found
            if(root.left == null && root.right==null) { // If it is leaf noode
                root = null; // Just set the node to null
            } 
            else if(root.left == null) { // If left node is null
                root = root.right; // Swap the right node to current node
            } 
            else if(root.right == null){ // if right node is null
                root = root.left; // Swap the left node to current node
            } 
            else { // If node has two children
                // When the node has two children, we simply cannot delete the node
                // We have to maintain the property of BST

                // To maintain the property of BST, we need too find the next element 
                // which occurrs in BST. That can be found by
                // finding left most value of the right sub tree

                // Find next successor
                TreeNode successor = root.right; 
                while(successor.left != null) {
                    successor = successor.left;
                }
                // After we found the successor. we assign the current node val to
                // the succcessor val
                root.val = successor.val;
                // Then we need to remove the node minimum value from right sub tree
                // Hence we delete it 
                root.right = deleteNode(root.right, successor.val);
            }
        }
        return root;
    }
}
```
