# Core Concept

Heap is a data  structure in which either max or min is on the top of the binary tree. Priority queue is an abstract data type it can be implemented by using heap.

According to Wikipedia, a **Heap** is a special type of binary tree. A heap is a binary tree that meets the following criteria:

* Is a **complete binary tree**;
* The value of each node must be **no greater than (or no less than)** the value of its child nodes.

A Heap has the following properties:

* Insertion of an element into the Heap has a time complexity of `O(logN)`
* Deletion of an element from the Heap has a time complexity of `O(logN)`
* The maximum/minimum value in the Heap can be obtained with `O(1)` time complexity.

Classification of Heap

There are two kinds of heaps: **Max Heap** and **Min Heap**.

* Max Heap: Each node in the Heap has a value **no less than** its child nodes. Therefore, the top element (root node) has the **largest** value in the Heap.
* Min Heap: Each node in the Heap has a value **no larger than** its child nodes. Therefore, the top element (root node) has the **smallest** value in the Heap.<br>

<figure><img src="https://leetcode.com/explore/featured/card/Figures/heap_explore/1_1_min_max_heap_diagram_new.png" alt=""><figcaption></figcaption></figure>
