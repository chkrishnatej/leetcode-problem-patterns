# Study Required

#### 1. Combinatorics and Factorial Math

Understanding the "size" of the permutation space is critical for L5 efficiency.

* **Factorial Number System (Factoradic):** Essential for finding the $$k^{th}$$ permutation. It maps a decimal number to a unique representation where each digit's place value is a factorial ($$n!$$).
* **Inversion Pairs:** The number of pairs $$(i, j)$$ such that $$i < j$$ and $$arr[i] > arr[j]$$. This is the mathematical basis for calculating the "distance" between two permutations.
* **Pigeonhole Principle:** Used in "Permutation Sequence" variants to determine digit availability.

#### 2. Monotonic Data Structures

These provide the $$O(n)$$ backbone for "Next Greater/Smaller" variants.

* **Monotonic Stack:** Used to find the nearest greater/smaller element by maintaining a strictly increasing or decreasing order of elements yet to be resolved.
* **Monotonic Deque:** Used when the window of visibility moves (Sliding Window Maximum).

#### 3. Lexicographical Logic

The formal rules governing dictionary order.

* **Pivot/Successor/Suffix Logic:** The three-step greedy algorithm for incrementing or decrementing a sequence with minimal change.
* **Total Ordering:** Understanding how the algorithm changes when you have non-comparable elements or custom sort orders.

#### 4. Advanced Data Structures for Large-Scale Variants

When the array is too large for $$O(n)$$ or requires frequent updates.

* **Fenwick Tree (Binary Indexed Tree) or Segment Tree:** Used to count "available" numbers in $$O(\log n)$$ when building the $$k^{th}$$ permutation. This avoids $$O(n^2)$$ complexity.
* **Balanced BST (TreeMap in Java):** Useful for maintaining a sorted suffix that allows for dynamic insertions and $$O(\log n)$$ "Successor" searches.

#### 5. String Manipulation & Digit DP

Permutations are often represented as strings or large integers.

* **Backtracking with Pruning:** To generate permutations that satisfy specific constraints (e.g., must be divisible by $$ $X$ $$).
* **Digit Dynamic Programming:** A technique used to count permutations or numbers within a range that satisfy a property without generating them.

#### 6. Symmetry in Algorithms

L5 interviews often ask you to "reverse" a solution.

* **Previous Permutation:** Requires inverting the "Next Permutation" logic (identifying the first decrease from the right).
* **Previous Greater Element:** Symmetrical to NGE; requires scanning from left-to-right or inverting the stack logic.

***

#### Learning Path Summary

| **Priority** | **Topic**             | **Application**                                            |
| ------------ | --------------------- | ---------------------------------------------------------- |
| High         | Monotonic Stack       | NGE, Daily Temperatures, Histogram                         |
| High         | Pivot-Swap-Reverse    | Next/Previous Permutation                                  |
| Medium       | Factoradic Math       | K-th Permutation                                           |
| Medium       | Fenwick/Segment Trees | Optimized $$ $k^{th}$ $$ Permutation ($$ $O(n \log n)$ $$) |
| Advanced     | Digit DP              | Constrained Permutations                                   |
