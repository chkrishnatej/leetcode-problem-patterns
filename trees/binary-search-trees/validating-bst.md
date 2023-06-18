# Validating BST

{% hint style="info" %}
Time Complexity: `O(n)`

> Traverses through all the nodes
{% endhint %}

{% hint style="success" %}
Space complexity: `O(h)`

> `` `h` `` is the height of the binary tree.&#x20;
>
> This is because we make recursive calls to traverse the left and right subtrees, and the maximum depth of the recursive call stack is equal to the height of the binary tree.
>
> &#x20;In the worst case, where the binary tree is a skewed tree, the height h can be equal to n, the number of nodes in the tree, which makes the space complexity O(n).&#x20;
>
> However, in a balanced binary tree, the height h is O(log n), which makes the space complexity O(log n).
{% endhint %}

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, null, null);
    }
    
    private boolean isValidBST(TreeNode root, Integer min, Integer max) {
        // Reached the leaf node, which means property of BST is satisfied all along
        if(root == null) {
            return true;
        }
        
        // Invalidating condition for BST
        // left subtree value should be less than root value
        // right subtree value should be greater than root value
        if((min!=null && root.val >= min) || (max!=null && max <= root.val)) {
            return false;
        }

        // Iterate it over left and right subtrees
        return isValidBST(root.left, min, root.val) && isValidBST(root.right, root.val, max);
    }
}
```
