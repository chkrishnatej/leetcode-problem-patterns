# InOrder Traversal

{% tabs %}
{% tab title="Recursive" %}
## Time and Space Complexity

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inOrder(root, result);
        return result;
    }
    
    private void inOrder(TreeNode node, List<Integer> result) {
        if(node==null)
            return;
        
        inOrder(node.left, result); // Traverses through left nodes
        result.add(node.val); // Processing root, by adding to result
        inOrder(node.right, result); // Traverses through right nodes
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
public List<Integer> inorderTraversal(TreeNode root) {
    // Storing the result
    List<Integer> result = new ArrayList<>();
    
    // Handling null case
    if (root == null) {
        return res;
    }
    // Processor queue
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;
    
    while (curr != null || !stack.isEmpty()) { // Termination condition
        // Traverse through the left node until it is a leaf node
        while (curr != null) { 
            stack.push(curr); 
            curr = curr.left;
        }
        // Pop the top node to process
        curr = stack.pop();
        // Add the node value to result
        result.add(curr.val);
        // Set the current node to process the right elements
        curr = curr.right;
    }
    return result;
}

```
{% endtab %}
{% endtabs %}
