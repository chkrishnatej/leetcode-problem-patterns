# Binary Search Trees

A **binary search tree (BST)**, a special form of a binary tree, satisfies the binary search property:

1. The value in each node must be <mark style="background-color:red;">**greater than (or equal to)**</mark> any values stored in its left subtree.
2. The value in each node must be <mark style="background-color:red;">**less than (or equal to)**</mark> any values stored in its right subtree.

## Important Properties of BST

1. <mark style="background-color:red;">InOrder traversal</mark> is not an unique identifier for BST
2. <mark style="background-color:green;">PreOrder</mark> and <mark style="background-color:green;">PostOrder</mark> are unique identifiers of BST
3. `inOrder = sorted(preOrder) + sorted(postOrder)`
4. <mark style="background-color:purple;">InOrder + PreOrder</mark> and <mark style="background-color:purple;">InOrder + PostOrder</mark> are unique identifiers of BST and be used to reconstruct the BST

