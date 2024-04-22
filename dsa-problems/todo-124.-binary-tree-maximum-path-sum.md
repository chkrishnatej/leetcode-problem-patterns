---
description: '#dfs #tree'
---

# TODO: 124. Binary Tree Maximum Path Sum

## Problem

{% embed url="https://leetcode.com/problems/binary-tree-maximum-path-sum/" %}

## Intuition

<figure><img src="../.gitbook/assets/image (42).png" alt="" width="375"><figcaption><p>Original Tree</p></figcaption></figure>



![](<../.gitbook/assets/image (23).png>)&#x20;

In the above tree, with node 3

> maxLeft = 4
>
> maxRight = 5

Before we can pick a node to make a path, we should pick the node which maximises the path value

$$
maxLeftRight = max(maxLeft, maxRight)
$$

> max(3+4(node 4), 3+5(node 5) = node(5)&#x20;

<img src="../.gitbook/assets/image (41).png" alt="" data-size="original">&#x20;

So 5 is picked and added to node 3, so the value of 5 is pushed upwards to node 3&#x20;

$$
maxRootLeftRight = max(root.val, root.val+maxLeftRight)
$$

![](<../.gitbook/assets/image (33).png>)

Here in this binary tree we should check if the full binary tree val is larger than root value

$$
maxAll = max(maxRootLeftRight, root.val)
$$

In the above case, since all are positive ints it will pick maxValue as maxRootLeftRight

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption><p>maxRootLeftRight is max value</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p>root.val is max</p></figcaption></figure>

## Time Complexity



## Space Complexity



## Solution

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
    private int maxSum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        // TC: O(n), traversing every node once
        // SC: O(h), height of the stack
        if(root == null) {
            return 0;
        }
        getPathSum(root);
        return maxSum;
    }

    private int getPathSum(TreeNode node) {
        if(node == null) return 0;

        // Pick the max of 0 or pathsum. We are using 0 because, if already a negative
        // value is present, then adding to current value only makes the value lower
        int leftPathSum = Math.max(0, getPathSum(node.left));
        int rightPathSum = Math.max(0, getPathSum(node.right));

        // In case, if negative value is the highest path sum, it is compared  to maxSum
        // so negative values are also not missed
        // Take the total sum value of the path
        int currSum = leftPathSum + node.val + rightPathSum;
        maxSum = Math.max(maxSum, currSum); // Keep comparing to maxSum at every iteration
        
        // The reason we are choosing max of leftPathSum or rightPathSum is, according
        // to the question, we need to pick a path which maximises the sum
        return node.val + Math.max(leftPathSum, rightPathSum);
    }
}
```
