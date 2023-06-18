# Types of Logics to solve Linked Lists

Linked Lists problems can be broadly solved using these four techniques:

## Reversal of Linked List

* It helps in understanding how to rearrange the nodes and links

```java
public void reverseLinkedList(ListNode head) {
    ListNode prev = null;
    ListNode current = head;
    ListNode forw = null;
    
    while(current != null) {
        forw = current.next; // Before de-linking, use this as reference
        current.next = prev; // Reversing the link
        // Moving the assignments
        prev = current; // Since the current node link is rearranged, set prev as current node
        current = forw; // And current will be processed, so set current as unlinked forw
    }
    
    return prev;
}
```

* **Time Complexity**: `O(n)`, n-> number of nodes in the linked list
* **Space Complexity:** `O(1)`, no extra space is used.

#### Problems to be solved:

* Reverse Linked List --> [`https://leetcode.com/problems/reverse-linked-list/`](https://leetcode.com/problems/reverse-linked-list/)
* Reverse Linked List II --> [`https://leetcode.com/problems/reverse-linked-list-ii/`](https://leetcode.com/problems/reverse-linked-list-ii/)

<img src="../.gitbook/assets/file.excalidraw (2).svg" alt="" class="gitbook-drawing">

## Two pointer approach

In this approach, two pointers will move forward at different speeds. This is also called as rabbit and tortoise approach.

We have two pointers `slow` that traverse one node at a time and `fast` generally traverse two nodes at a time.&#x20;

**But make sure the `while` loop termination is done correctly**

{% code title="TerminalCondition" %}
```java
while(runner!=null && runner.next!=null)
```
{% endcode %}

`runner` should always be able to access the nodes two points away from the current. &#x20;

{% tabs %}
{% tab title="Iterative" %}
### Time Complexity:

$$O(n)$$ n--> number of nodes in LinkedList

### Space Complexity

$$O(1)$$ auxillary space. Didn't use any new structure to store things

{% code title="MiddleNode.java" %}
```java
public ListNode middleNode(ListNode head) {
        // Time complexity: O(n), n-> number of nodes in LinkedList
        // Space complexity: O(1), auxillary space. Didn't use any new structure to store things
        
        ListNode walker = head;
        ListNode runner = head;

        while(runner!=null && runner.next!=null) {
            walker = walker.next;
            runner = runner.next.next;
        }
        return walker;
    }
```
{% endcode %}
{% endtab %}

{% tab title="TODO: Recursive" %}

{% endtab %}
{% endtabs %}

#### Problems to be solved

* Finding Middle of Linked Lists --> [https://leetcode.com/problems/middle-of-the-linked-list/](https://leetcode.com/problems/middle-of-the-linked-list/)
* Detecting cycle in LinkedList --> [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)
* Detecting the start of the cycle in Linked List --> [https://leetcode.com/problems/linked-list-cycle-ii/](https://leetcode.com/problems/linked-list-cycle-ii/)
* Get Intersection of Linked List   --> [https://leetcode.com/problems/intersection-of-two-linked-lists/](https://leetcode.com/problems/intersection-of-two-linked-lists/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       &#x20;

<img src="../.gitbook/assets/file.excalidraw (2).svg" alt="" class="gitbook-drawing">

## Merge Linked List

Merging two linked lists based on a property or a condition.

### Merging two sorted lists

{% tabs %}
{% tab title="Iterative" %}
### Time Complexity

Because exactly one of `l1` and `l2` is incremented on each loop iteration, the `while` loop runs for a number of iterations equal to the sum of the lengths of the two lists. All other work is constant, so the overall complexity is linear.

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    // n -> no.of nodes in l1
    // m -> no.of nodes in l2
    // Time complexity: O(n+m)
    // Space complexity:O(1), Auxillary space complexity
    if(l1 == null || l2 == null) { // If any of the list is null
        return l1 == null ? l2 : l1; // Return the other list
    }
    
    ListNode dummy = new ListNode(0); // Creating a dummy node
    ListNode head = dummy; // Assigning dummy node to head
    
    while(l1 != null && l2 != null) {
        if(l1.val <= l2.val) { // Condition two check sorted order
            // Asssign the lowest value node b/w l1 and l2 to dummy.next
            dummy.next = l1; 
            // Move forward with l1 node, as the current one is processed
            l1 = l1.next; 
        } else {
            // The above inference can be implied here too
            dummy.next = l2;
            l2 = l2.next;
        }
        // Move forward with dummy, so that the l1 and l2 nodes are appended
        dummy = dummy.next;
    }
    // Check if any list is not yet processed, then append them to dummy
    dummy.next = l1 != null ? l1 : l2;
    
    // In the beginning, we assign he head as dummy pointer
    // The first node of head is dummy(0), so we need to return the head.next
    return head.next;
}
```
{% endtab %}

{% tab title="Recursive" %}
### Time Complexity:

Because each recursive call increments the pointer to `l1` or `l2` by one (approaching the dangling `null` at the end of each list), there will be exactly one call to `mergeTwoLists` per element in each list. Therefore, the time complexity is linear in the combined size of the lists.

### Space Complexity

The first call to `mergeTwoLists` does not return until the ends of both `l1` and `l2` have been reached, so $$n+m$$ stack frames consume  $$O(n+m)$$ space.



```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // n -> no.of nodes in l1
        // m -> no.of nodes in l2
        // Time Complexity: O(n+m)
        // Space Complexity: O(n+m)
        if(l1 == null || l2 == null) {
            return l1==null ? l2 : l1;
        }
        
        if(l1.val <= l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
```
{% endtab %}
{% endtabs %}

### Problems to be Solved

* Merging two sorted lists --> [https://leetcode.com/problems/merge-two-sorted-list](https://leetcode.com/problems/merge-two-sorted-list)
*

### Sentinel/Dummy Node
