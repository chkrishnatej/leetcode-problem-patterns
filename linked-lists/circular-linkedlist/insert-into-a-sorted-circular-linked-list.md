# 😐 Insert into a Sorted Circular Linked List

### Problem:

`https://leetcode.com/problems/insert-into-a-sorted-circular-linked-list/`

![](<../../.gitbook/assets/image (19) (1).png>)![](<../../.gitbook/assets/image (18) (1).png>)

![](<../../.gitbook/assets/image (8) (1).png>)



### Understanding of the problem

The problem on a higher level looks simple. It can be divided into two parts

* &#x20;Figure out the insertion position&#x20;
* Rearranging the node links

But there could be edge cases as, it is a circular linked list; you might need to add where the circular link is happening, i.e, where the tail is being linked to head

And to make sure of while loop working correctly, I thought of adding a `HashSet` to keep track of nodes that have been visited. It adds space complexity. Instead,lin you can use the condition, `while(node.next!=head)`&#x20;

### Solution

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node next;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _next) {
        val = _val;
        next = _next;
    }
};
*/

class Solution {
    public Node insert(Node head, int insertVal) {
        // Time complexity: O(n), n=> number of nodes
        // Space complexity: Auxillary space
        
        // Edge case
        if(head==null) {
            Node newNode = new Node(insertVal);
            newNode.next = newNode;
            return newNode;
        }

        Node current = head;
        while(current.next != head) {
            if(current.val <= current.next.val) { // When traversing forward, regulaar case
                // Figure out in between
                if(current.val <= insertVal && insertVal <= current.next.val) {
                    break;
                }
            } else { // When circular link is there
                // Check if insertval should be between first and last
                if(current.val <= insertVal || insertVal <= current.next.val) {
                    break;
                }
            }
            current = current.next;
        }

        // Rearranging the links
        Node newNode = new Node(insertVal,current.next);
        current.next = newNode;

        return head;
    }
}
```



