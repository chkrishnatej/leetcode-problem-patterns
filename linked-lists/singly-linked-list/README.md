# Singly Linked List

A singly linked list is a chained data structure where a node holds data and addresses it to the next node sequentially

<figure><img src="../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

### Inserting First in singly linked List

<figure><img src="../../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

### Code

```java
public void insertFirst(int data) {
        Node newNode = new Node(data);
        newNode.next = head; // Setting current new node address pointer next to previous head
        head = newNode; // Assigning the current new node as the head
    }
```

## Inserting Last in Singly Linked List

<figure><img src="../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

### Code

```java
public void insertLast(int data) {
        Node current = head; // Assign current as head node
        Node newNode = new Node(data); // Create a new node
        if(current == null) { // If SLL is empty,
            head = newNode; // Head Node is the new node
        } else { // If head node is not empty
            while(current.next != null) {  // Get to the last
                current = current.next;
            }
            current.next = newNode; // Assign new node as last node next
        }
    }
```
