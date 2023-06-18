# Convert sorted array into BST

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

Read the important properties of BST in the above page

## Problem

[https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree)

## Solution

> n -> Length of input array

{% hint style="info" %}
### Time Complexity = O(n)
{% endhint %}

{% hint style="success" %}
### Space Complexity = O(log n)
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
    private int[] nums;

    public TreeNode sortedArrayToBST(int[] nums) {
        // n -> Length of input array
        // Time Complexity: O(n)
        // Space Complexity: O(log n), recursion stack
        this.nums = nums;
        return helper(0, nums.length-1);
    }

    private TreeNode helper(int left, int right) {
        // Base case to maintain right height balanced tree
        if(left > right) {
            return null;
        }
        // Pick a left middle number
        int mid = (left + right)/2;

        // Create a node with left mid
        TreeNode root = new TreeNode(nums[mid]);
        // Recurisvely build left and right subtrees
        root.left = helper(left, mid - 1); // Left to mid-1 is for left subtree
        root.right = helper(mid + 1, right); // mid+1 to right is for right subtree
        
        return root;
    }
}
```
