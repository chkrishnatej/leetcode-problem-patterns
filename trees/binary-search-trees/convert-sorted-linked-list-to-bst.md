# Convert Sorted Linked List to BST

## Problem

{% embed url="https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/description/" %}

## Time and Space Complexity

> n -> Number of nodes in Linked List

{% hint style="info" %}
### Time Complexity:

`O(N log N)`

#### Explanation:

* The time complexity of building a BST is **`O(NlogN)`**.&#x20;
* The time complexity of merging two BSTs is **`O(N)`**.&#x20;
* Therefore, the overall time complexity of your algorithm is **`O(NlogN)`**.
{% endhint %}

{% hint style="success" %}
### Space Complexity

`O(log n)`

* Space taken by recursion stack
{% endhint %}

## Solution

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
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
    public TreeNode sortedListToBST(ListNode head) {
        /*
        n -> Number of nodes in head listnode
        Time Complexity: O(n logn)
            * The time complexity of building a BST is O(NlogN).
            * The time complexity of merging two BSTs is O(N).
            * Therefore, the overall time complexity of your algorithm is O(NlogN).

        Space complexity: O(log n), recursion stack
        */
        if(head==null) {
            return null;
        }
        return helper(head, null);
    }

    private TreeNode helper(ListNode head, ListNode tail) {
        ListNode slow = head;
        ListNode fast = head;
        // If head == tail, we cannot find the middle, so we return null
        if(head == tail) {
            return null;   
        }
        // Finding middle of LinkedList given head and tail
        while(fast != tail && fast.next != tail) {
            fast = fast.next.next;
            slow = slow.next;
        }

        // Create a node with middle element
        TreeNode root = new TreeNode(slow.val);
        // To construct left subtree, traverse between head and slow node
        root.left = helper(head, slow);
        // To construct right subtree, traverse between slow.next and tail
        // Use slow.next because, we already created node with slow
        root.right = helper(slow.next,tail);
        return root;
    }
}
```
