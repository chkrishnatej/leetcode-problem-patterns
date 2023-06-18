# Post Order Traversal

{% tabs %}
{% tab title="Recursive" %}
## Time and Space complexity

{% hint style="success" %}
* Time Complexity: `O(n)`
* &#x20;Space Complexity: `O(n)`

`n` --> Number of nodes
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
class TreeTraversal {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postOrder(root, result);
        return result;
    }
    
    private void postOrder(TreeNode node, List<Integer> result) {
        if(node==null)
            return;
        
        postOrder(node.left, result);
        postOrder(node.right, result);
        result.add(node.val);
    }
}
```
{% endtab %}

{% tab title="Iterative" %}
## Time and Space Complexity

{% hint style="success" %}
* Time Complexity: `O(n)`
* &#x20;Space Complexity: `O(n)`

`n` --> Number of nodes
{% endhint %}

```java
public List<Integer> postorderTraversal(TreeNode root) {
    // Use a linked list to store the result in reverse order
    LinkedList<Integer> result = new LinkedList<>(); 
    
    // If the root is null, return an empty list
    if (root == null) { 
        return result;
    }
    
    // Use a deque as a stack to traverse the tree
    Deque<TreeNode> stack = new ArrayDeque<>(); 
    stack.push(root); // Push the root node onto the stack

    while (!stack.isEmpty()) {
        // Pop the top node from the stack for processing
        TreeNode node = stack.pop(); 
        
        // Add the node value to the front of the linked list (to reverse the order)
        result.addFirst(node.val); 
        
        // Push the left child onto the stack if it exists
        if (node.left != null) { 
            stack.push(node.left);
        }
        // Push the right child onto the stack if it exists
        if (node.right != null) { 
            stack.push(node.right);
        }
    }
    
    // Return the linked list as the final result in postorder traversal
    return result; 
}

```
{% endtab %}
{% endtabs %}
