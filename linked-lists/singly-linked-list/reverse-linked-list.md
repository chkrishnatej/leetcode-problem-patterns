# 😐 Reverse Linked List

### Problem

[`https://leetcode.com/problems/reverse-linked-list/`](https://leetcode.com/problems/reverse-linked-list/)

<figure><img src="../../.gitbook/assets/Screenshot 2023-04-13 at 8.20.54 PM.png" alt=""><figcaption></figcaption></figure>

### Understanding of the problem

We take the three-pointers `prev` , `current` and `forw`. These pointers help in linking and unlinking the nodes

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

### Edge cases

* Null linked list
* Linked list with only one node

### Solution

````java
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
class Solution {
    public ListNode reverseList(ListNode head) {
        // Time complexity: O(n), n->number of nodes in linked list
        // Space complexity: O(1), Auxillary space
        
        // Edge case
        if(head==null || head.next==null){
            return head;
        }
        ListNode prev = null;
        ListNode curr = head;
        ListNode forw = null;

        while(curr != null) {
            // Keeps track of next node to be processed after link is broken
            forw = curr.next; 
            // Set the current node link to prev (Reverse happens here). This breaks the link to
            // next pointer
            curr.next = prev; 
            // Assigning current node to prev.
            prev = curr;
            // Assigning next node to be processed to current
            curr = forw;
        }

        // After all the reversal happens the previous node will be having reverse links
        return prev;
    }
}
```
````
