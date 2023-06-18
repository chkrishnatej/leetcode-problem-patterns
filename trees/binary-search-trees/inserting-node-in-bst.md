# Inserting Node in BST



{% tabs %}
{% tab title="Recursive" %}
> n -> Number of nodes in tree

{% hint style="info" %}
### Time Complexity

* Worst case (skewed): `O(n)`
* Average case: `O(log n)` or `O(H)`
{% endhint %}

{% hint style="success" %}
### Space Complexity

* Worst case (skewed): `O(n)`
* Average case: `O(log n)` or `O(H)`
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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        
        // Space Complexity:
        //  --> Worst case(skewed): O(n)   
        //  --> General case: O(H) or O(log n)
        if(root == null) {
            return new TreeNode(val);
        } else if(val < root.val) {
            root.left = insertIntoBST(root.left, val);
        } else {
            root.right = insertIntoBST(root.right, val);
        }
        return root;
    }
}va
```
{% endtab %}

{% tab title="Iterative" %}
{% hint style="info" %}
### Time Complexity:

* Best Case: `O(log n)`, when it is a balanced binary search tree&#x20;
* Worst Case: `O(n)`, when it is a skewed binary search tree
{% endhint %}

{% hint style="success" %}
### Space Complexity =\``` O(1)` ``
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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        /*
            n -> Number of nodes in BST
            Time Complexity:
                * Best Case: O(log n), when it is a balanced binary search tree
                * Worst Case: O(n), when it is a skewed binary search tree
            Space Complexity: O(1) 
        */

        // If no node is present, create a new node
        if(root==null) {
            return new TreeNode(val);
        }


        TreeNode curr = root;
        
        // Infinite loop
        while(true) {
            // If target is less than current node value,
            // search towards left
            if(val < curr.val) { 
                // If there is existing left node, traverse through it
                if(curr.left != null) {
                    curr = curr.left;
                } 
                // If not, add the new node to current node left
                else {
                    curr.left = new TreeNode(val);
                    break; // After inserting, we can break from the infinite loop
                }
            } 
            // If target is greater than current node value,
            // search towards right
            else if(val > curr.val) {
                // If there is existing right node, traverse through it
                if(curr.right != null){
                    curr = curr.right;
                } 
                // If not, add the new node to current node right
                else {
                    curr.right = new TreeNode(val);
                    break; // After inserting, we can break from the infinite loop
                }
            }
        }

        // return the same tree
        return root;
    }
}
```
{% endtab %}
{% endtabs %}





