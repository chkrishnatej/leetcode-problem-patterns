# Disjoint Sets - Union Find (Connected Components)

Disjoint Set is an abstract data structure. It helps find the connected components in a dynamic graph in <mark style="background-color:green;">near constant time</mark>.&#x20;

A dynamic graph is a graph that keeps on changing the configuration—for example, a network connection.

You can read more about dynamic graph here -->[ Dynamic Graph Striver](https://takeuforward.org/data-structure/disjoint-set-union-by-rank-union-by-size-path-compression-g-46/)

Disjoint set mainly has two types of operations:

* Finding the parent of the node
* Union (creating an edge between two nodes)
  * Union by Rank
  * Union by Size

## Terminology

> **Disjoint Set** --> Disjoint set is a data structure that keeps track of a collection of disjoint (non-overlapping) sets.

> **Size** --> Number of elements in a set or subset

> **Path compression** --> A technique used to improve efficiency of the disjoint set. It works by making the path from a node to its root as short as possible. This is done by recursively changing the parent of each node to be its grandparent.&#x20;

> **Rank** --> Rank of node refers to the distance between furthest and current node

> **Parent** --> Node right above the current node

> **Ultimate parent** --> Ultimate parent refers to the topmost or root node of the current node

### Path compression

If we have to find the ultimate parent, we have to traverse through the nodes connected to it. And it takes `O(log n)` time. But using path compression, we connect all the nodes to the ultimate parent directly.&#x20;

This helps in retrieving the ultimate parent in almost constant time.

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

## Time and Space complexity

| Algorithm     | Time complexity | Space complexity |
| ------------- | --------------- | ---------------- |
| Union by rank | O(α(n))         | O(n)             |
| Union by size | O(α(n))         | O(n)             |

Where **`α(n)`** is the _inverse Ackermann function,_ which is a very slowly growing function. In practice, both algorithms are essentially **`O(n)`**



## Union By Rank

Union by rank is a technique to combine disjoint sets. <mark style="background-color:green;">The idea is to combine the set with the smaller rank with the set with the larger rank, so that the resulting set has the largest possible rank</mark>. This can be done by maintaining a list of the ranks of all the sets, and then always combining the two sets with the smallest ranks.

Union by rank is a simple and efficient way to combine disjoint sets. It is also relatively easy to implement, and it can be used to solve a variety of problems, such as finding the connected components of an undirected graph.

### Algorithm

1. Initialize a list of parents and a list of ranks.
2. For each node, set its parent to itself and its rank to `-1`.
3. To find the representative of the set that a given node belongs to:
   1. If the node is its own parent, then it is the representative of its set.
   2. Otherwise, recursively call find on the parent of the node.
4. To unite the sets that two given nodes belong to:
   1. Find the representatives of the sets that the two nodes belong to.
   2. If the two nodes belong to the same set, then do nothing.
   3. Find the rank of the two sets.
   4. Unite the two sets by making the smaller set a child of the larger set.
   5. If the rank of the two sets is the same, then increase the rank of the larger set by 1.

## Code

```java
public class DisjointSet {

  // Each node in the list points to its parent.
  private List<Integer> parent;

  // The rank of a set is the number of nodes in its subtree.
  private List<Integer> rank;

  // Constructor. Creates a disjoint set with n nodes. This is generally makeset
  public DisjointSet(int n) {
    // Initialize the parent list and the rank list.
    parent = new ArrayList<>();
    rank = new ArrayList<>();

    // For each node, set its parent to itself and its rank to 10.
    for (int i = 0; i < n; i++) {
      parent.add(i);
      rank.add(-1);
    }
  }

  // Finds the representative of the set that the given node belongs to along with path compression
  public int findParent(int node) {
    // If the node is its own parent, then it is the representative of its set.
    if (node == parent.get(node)) {
      return node;
    }

    // Otherwise, recursively call find on the parent of the node.
    int parentNode = findParent(parent.get(node));

    // Update the parent of the node to point to the representative of its set.
    parent.set(node, parentNode);
    return parent.get(node);
  }

  // Unites the sets that the given two nodes belong to.
  public void unionByRank(int x, int y) {
    // Find the representatives of the sets that the two nodes belong to.
    int parentX = findParent(x);
    int parentY = findParent(y);

    // If the two nodes belong to the same set, then do nothing.
    if (parentX == parentY) {
      return;
    }

    // Find the rank of the two sets.
    int rankX = rank.get(parentX);
    int rankY = rank.get(parentY);

    // Unite the two sets by making the smaller set a child of the larger set.
    if (rankX < rankY) {
      // Set the parent of the smaller set to the larger set.
      parent.set(parentX, parentY);
    } else if (rankY < rankX) {
      // Set the parent of the larger set to the smaller set.
      parent.set(parentY, parentX);
    } else {
      // Set the parent of the smaller set to the larger set.
      parent.set(parentX, parentY);
      // Increase the rank of the larger set by 1.
      rank.set(parentX, rankX + 1);
    }
    return;
  }
}
```

## Union By Size

Union by size is a technique to combine disjoint sets. <mark style="background-color:green;">The idea is to combine the smaller set with the larger set, so that the resulting set is as large as possible</mark>. This can be done by maintaining a list of the sizes of all the sets, and then always combining the two sets with the smallest sizes.

Union by size is a simple and efficient way to combine disjoint sets. It is also relatively easy to implement, and it can be used to solve a variety of problems, such as finding the connected components of an undirected graph.

### Algorithm

1. Initialize a list of parents and a list of sizes.
2. For each node, set its parent to itself and its size to **1**.
3. To find the representative of the set that a given node belongs to:
   1. If the node is its own parent, then it is the representative of its set.
   2. Otherwise, recursively call find on the parent of the node.
4. To unite the sets that two given nodes belong to:
   1. Find the representatives of the sets that the two nodes belong to.
   2. If the two nodes belong to the same set, then do nothing.
   3. Find the size of the two sets.
   4. Unite the two sets by making the smaller set a child of the larger set.
   5. If the size of the two sets is the same, then set the parent of the smaller set to the larger set.

### Code

```java
public class DisjointSet {

  // Each node in the list points to its parent.
  private List<Integer> parent;

  // The size of a set is the number of nodes in the set.
  private List<Integer> size;

  // Creates a disjoint set with n nodes. This generally the makesse
  public DisjointSet(int n) {
    // Initialize the parent list and the size list.
    parent = new ArrayList<>();
    size = new ArrayList<>();

    // For each node, set its parent to itself and its size to 1.
    for (int i = 0; i < n; i++) {
      parent.add(i);
      size.add(1);
    }
  }

  // Finds the representative of the set that the given node belongs to
  public int findParent(int node) { // Does path compression also
    // If the node is its own parent, then it is the representative of its set.
    if (node == parent.get(node)) {
      return node;
    }

    // Otherwise, recursively call find on the parent of the node.
    int parentNode = findParent(parent.get(node));

    // Update the parent of the node to point to the representative of its set.
    parent.set(node, parentNode);
    return parent.get(node);
  }

  // Unites the sets that the given two nodes belong to.
  public void unionBySize(int x, int y) {
    // Find the representatives of the sets that the two nodes belong to.
    int parentX = findParent(x);
    int parentY = findParent(y);

    // If the two nodes belong to the same set, then do nothing.
    if (parentX == parentY) {
      return;
    }

    // Find the size of the two sets.
    int sizeX = size.get(parentX);
    int sizeY = size.get(parentY);

    // Unite the two sets by making the smaller set a child of the larger set.
    if (sizeX < sizeY) {
      // Set the parent of the smaller set to the larger set.
      parent.set(parentX, parentY);
      
      // Increase the size of the larger set by the size of the smaller set.
      size.set(parentY, sizeX + sizeY);
    } else {
      // Set the parent of the larger set to the smaller set.
      parent.set(parentY, parentX);

      // Increase the size of the smaller set by the size of the larger set.
      size.set(parentX, sizeX + sizeY);
    }
    return;
  }
}
```
