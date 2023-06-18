# Pre Order Traversal

{% tabs %}
{% tab title="Recursive" %}
### Time and Space Complexity

{% hint style="success" %}
* Time Complexity: `O(n)`
* &#x20;Space Complexity: `O(n)`

`n` --> Number of nodes
{% endhint %}

### Code

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
class TreeTraversal {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        
        if(root==null)
            return result;
        
        preOrder(root, result);
        return result;
    }
    
    private void preOrder(TreeNode node, List<Integer> result) {
        if(node == null)
            return;
        result.add(node.val);
        preOrder(node.left, result);
        preOrder(node.right, result);
    }
}
```
{% endtab %}

{% tab title="Iterative" %}
### Time and Space Complexity

{% hint style="success" %}
* Time Complexity: `O(n)`
* &#x20;Space Complexity: `O(n)`

`n` --> Number of nodes
{% endhint %}

{% hint style="danger" %}
Add the right nodes first to stack cause stack follows LIFO
{% endhint %}

### Code

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        // Initialize an empty result list to store the values of the tree nodes
        List<Integer> result = new ArrayList<>();
        // If the root is null, return the empty list
        if (root == null) {
            return result;
        }

        // Create a deque to store the nodes of the tree
        Deque<TreeNode> stack = new LinkedList<>();
        // Push the root node onto the stack
        stack.push(root);

        // Iterate until the stack is empty
        while (!stack.isEmpty()) {
            // Pop the top node from the stack and add its value to the result list
            TreeNode node = stack.pop();
            result.add(node.val);

            // Push the right child node onto the stack (if it exists) before the left child
            // so that the left child will be processed first (due to the LIFO nature of the stack)
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }

        // Return the result list
        return result;
    }
}
```
{% endtab %}
{% endtabs %}

