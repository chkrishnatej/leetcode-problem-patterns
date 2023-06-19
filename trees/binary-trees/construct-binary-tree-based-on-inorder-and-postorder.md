# Construct Binary Tree based on inorder and postorder

### Problem

{% embed url="https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/" %}

## Approach

We can construct binary tree when inorder and post order is given <mark style="background-color:green;">uniquely</mark>.&#x20;

> INORDER ==> LEFT --> ROOT --> RIGHT
>
> POSTORDER ==> LEFT --> RIGHT --> ROOT

Based on the above understanding, <mark style="background-color:green;">we can conclude that the last element of postorder will be the ROOT node.</mark>

The inorder helps in identifying the left subtree and right subtree

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption><p>InOrder Traversal</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption><p>PostOrder Traversal</p></figcaption></figure>

We create a map for inorder to store the maintain the position of elements. This helps in creating left and right subtree.

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

### Calculating left and right subtree boundaries in inOrder

* inStart = 0
* inEnd = inorder.length - 1

Based on the above `inStart`, first, we will calculate the left and the right subtree boundaries.

The root value of the binary tree is the last element of the **postorder** i.e. 3&#x20;

* index = map.get(root.val) = map.get(3) = 1

Based on the index retrieved from the map, we define boundaries for the left subtree and right subtree

* **For left subtree**
  * \[inStart, index - inStart - 1] ⇒ \[0, 0]
* **For right subtree**&#x20;
  * \[index + 1, inorder.length-1] ⇒ \[2, 4]

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

### Calculating left and right subtree boundaries in post order

* postStart = 0
* postEnd = postorder.length - 1

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

**For left subtree**

We know postorder will first have left nodes and also know the number of left subtree nodes from inorder.

* postEnd = currentPostStart + left subtree boundary from inorder\
  \=  0  + (index - inStart - 1)\
  \= 0 + (1 - 0 - 1) = 0 + 0 = 0

Left Subtree postorder boundaries = \[postStart, postEnd] ⇒ \[0, 0]

**For right subtree**

Since we know the number of left subtree nodes, the right subtree would be the boundary of left subtree in postorder + 1

* postStart = left subtree boundary in postorder + 1\
  \=  (index - inStart  - 1) + 1\
  \=  (1 - 0 - 1) + 1 = 1
* postEnd = postEnd - 1 = 4 - 1 = 3

Right Subtree boundaries = \[postStart, postEnd - 1] ⇒ \[1, 3]     &#x20;

### **Algorithm** &#x20;

* Create an inorder map which contans key as value of inorder in given index and value as index&#x20;
* Initialise `inStart` and `inEnd` for calculating boundaries of left and right subtrees
* Initialise `postStart` and `postEnd` for calculating boundaries of left and right subtrees
* Take the last element of postorder as root
* Find the position of that element in inorder using map
* Create a `TreeNode`
* Calculate left subtree boundaries
  * For inorder ==> \[inStart, index-1]&#x20;
  * For postorder ==> \[postStart, index - inStart -1]
* Calculate right subtree boundaries
  * For inorder ==> \[index+1, inOrder.length - 1]
  * For postorder ==> \[index-inStart-1, postEnd - 1]
* Do it recursivey

### Solution:

{% hint style="info" %}
### Time Complexity:

O(n)

It has to process all the nodes or elements in the inorder/postorder
{% endhint %}

{% hint style="success" %}
### Space Complexity

O(n) + O(log n)

n -> HashMap

log n --> Recursion stack
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
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        // n -> Length of inorder / postorder
        // Time Complexity: O(n)
        // Space Complexity: O(n) + O(log n), hashmap + recursion stack
        for(int i=0;i<inorder.length;i++) {
            map.put(inorder[i], i);
        }
        return build(inorder,0,inorder.length-1,postorder,0,postorder.length-1);
    }
    
    public TreeNode build(int[] inorder, int inStart, int inEnd, int[] postorder, int postStart, int postEnd){
        if(inStart > inEnd || postStart > postEnd) {
            return  null;
        }
        
        TreeNode root = new TreeNode(postorder[postEnd]);
        
        int index = map.get(postorder[postEnd]);
    
        root.left = build(inorder, inStart, index - 1, postorder, postStart, postStart + index - inStart - 1);
        root.right = build(inorder, index + 1, inEnd, postorder, postStart + index - inStart, postEnd - 1);
        
        return root;
    }
}
```
