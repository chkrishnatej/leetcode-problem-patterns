# Recover BST

{% tabs %}
{% tab title="Recursive" %}
> n -> Number of nodes in BST

{% hint style="info" %}
### Time Complexity:

* Best Case: `O(log n)`&#x20;
* Worst Case: `O(n)`, skewed BST
{% endhint %}

{% hint style="success" %}
### Space Complexity:

* Best Case: `O(log n)`&#x20;
* Worst Case: `O(n)`, skewed BST
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
    TreeNode first = null;
    TreeNode second = null;
    TreeNode prev = null;

    public void recoverTree(TreeNode root) {
        /*
            n -> Number of nodes in BST
            Time Complexity:
                * Best Case: O(log n)
                * Worst Case: O(n), skewed BST
            
            Space Complexity:
                * Best Case: O(log n)
                * Worst Case: O(n), skewed BST
        */

        // Perform inorder traversal and figure out the invariants
        inOrder(root);

        // Swap the value of first and second node values
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
        
        return;
    }

    private void inOrder(TreeNode node) {
        // Regular inorder traversal
        if(node==null) {
            return;
        }

        inOrder(node.left);

        // Since two nodes are swapped, there are two invariants
        // While performing inorder, BST should be giving us ascending sorted order

        // So, when prev val is not null AND first is not yet found
        // AND previous val >= currentNode.val breaks the property of BST
        // Then assign the first node value as previous, because the previous is 
        // greater value which broke BST property
        if(prev != null && first == null && prev.val >= node.val) {
            first = prev;
        }
        
        // Once first node is found, then again when the invariant occurs,
        // previous val >= currentNode.val, it should ideally be current node val
        // greater than previous value in inorder traversal
        // Since, the current node is what broke the BST property, we assign
        // the second node as current node
        if(first != null && prev.val >= node.val) {
            second = node;
        }

        // after processing, assign the previous node as current node
        prev = node;

        inOrder(node.right);
    }
}
```
{% endtab %}

{% tab title="Iterative" %}
> n -> Number of nodes in BST

{% hint style="info" %}
### Time Complexity:

* Best Case: `O(log n)`&#x20;
* Worst Case: `O(n)`, skewed BST
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
    public void recoverTree(TreeNode root) {
        /*
            Time Complexity:
                * Best case: O(log n)
                * Worst case: O(n), skewed BST
            
            Space Complexity: O(1)
        */
        if(root==null) {
            return;
        }

        // Trackers
        TreeNode first = null, second = null, prev = null;
        Deque<TreeNode> stack = new ArrayDeque<>();
        
        TreeNode node = root;
        // Standard iterative inorder traversal
        while(node!=null || !stack.isEmpty()) {
            while(node != null) {
                stack.push(node);
                node = node.left;
            }

            node = stack.pop();
            
            // Since two nodes are swapped, there are two invariants
            // While performing inorder, BST should be giving us 
            // ascending sorted order

            // So, when prev val is not null AND first is not yet found
            // AND previous val >= currentNode.val breaks the property of BST
            // Then assign the first node value as previous, because the previous is 
            // greater value which broke BST property
            if(prev != null && first == null && prev.val >= node.val) {
                first = prev;
            }

            // Once first node is found, then again when the invariant occurs,
            // previous val >= currentNode.val, it should ideally be current node val
            // greater than previous value in inorder traversal
            // Since, the current node is what broke the BST property, we assign
            // the second node as current node
            if(first != null && prev.val >= node.val) {
                second = node;
            }

            // after processing, assign the previous node as current node
            prev = node;

            node = node.right;
        }

        int temp = first.val;
        first.val = second.val;
        second.val = temp;

        return;        
    }
}
```
{% endtab %}
{% endtabs %}
