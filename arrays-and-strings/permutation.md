# Permutation

The general question that can be asked is "[Next Greatest Permutation](https://leetcode.com/problems/next-permutation)"

## Ways it can be twisted and asked

#### 1. K-th Permutation Sequence

Instead of the _next_ permutation, you are <mark style="background-color:purple;">asked for the</mark> $$k^{th}$$ <mark style="background-color:purple;">permutation of a set</mark> $$\{1, 2, \dots, n\}$$.

* **The Twist:** Repeatedly calling `nextPermutation` $$k$$ times results in $$O(k \cdot n)$$, which is too slow for large $$n, k$$.
* **The L5 Expectation: Use Factorial Number System (Factoradic) logic.** Calculate how many permutations start with each digit $$(n-1)!$$ and use $$k / (n-1)!$$ to pick digits greedily in $$O(n^2)$$ or $$O(n \log n)$$ with a Fenwick tree.

#### 2. Next Permutation with Constraints

The interviewer adds a constraint: "<mark style="background-color:purple;">Find the next permutation that is also a multiple of 7</mark>" or "the next permutation that is a palindrome."

* **The Twist:** The standard "pivot-swap-reverse" logic no longer guarantees the constraint.
* **The L5 Expectation:** You must pivot the discussion toward Backtracking with Pruning or Digit DP. You need to demonstrate that you recognize when a greedy $$O(n)$$ approach is no longer mathematically sufficient.

#### 3. Stream / Large Scale Data

"The permutation is too large to fit in memory" or "The digits are coming in a stream."

* **The Twist:** You cannot reverse the suffix or scan backward easily.
* **The L5 Expectation:** Discuss External Sorting or using a Balanced BST / Segment Tree to represent the sequence. If it's a stream, how do you maintain the "rightmost peak" in a data structure to answer "what is the next permutation" at any point?

#### 4. Permutation Distance (Rank)

"Given two permutations, how many steps are between them?"

* **The Twist:** This is the inverse of the K-th permutation problem.
* **The L5 Expectation:** Calculate the lexicographical rank of both permutations using the number of inversions or factoradic representation. The answer is $$|Rank(P_1) - Rank(P_2)|$$.

#### 5. Multi-set Permutations with Duplicates

"What if the array has many duplicates, and we want the next _unique_ permutation?"

* **The Twist:** The successor search must be strictly $$ $A[j] > A[i]$ $$, and the suffix reversal must handle the non-increasing (not strictly decreasing) property correctly.
* **The L5 Expectation:** Your code must already handle this (the `nums[i-1] >= nums[i]` and `nums[j] <= nums[pivot]` logic you wrote handles duplicates), but the interviewer will probe your understanding of why the equalities are placed where they are to avoid infinite loops or redundant swaps.

#### 6. Functional/Tree Variant

"Find the next greater node in a BST" or "Find the next permutation of a tree's leaf nodes."

* **The Twist:** The data isn't in a linear array.
* **The L5 Expectation:** Map the tree traversal (In-order) to the linear permutation problem. Demonstrate that the structure of the data (Tree vs Array) doesn't change the underlying lexicographical math.

#### 7. The "Previous" Permutation

"Find the largest permutation smaller than the current one."

* **The Twist:** Requires inverting the logic.
*   **The L5 Expectation**:&#x20;

    1\. Find the first $$A[i-1] > A[i]$$ from the right (increasing suffix).

    2\. Find the largest element in the suffix that is smaller than $$A[i-1]$$.

    3\. Swap and reverse the suffix.

***

#### Summary of L5 Probing Questions

| **Question**                                     | **Focus**                                                                                     |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------- |
| Can we do better than $$ $O(n)$ $$$$O(n)$$ time? | Testing your knowledge of the $$ $O(n)$ $$$$O(n)$$ bottleneck (reversal/pivot search).        |
| What if it's a circular buffer?                  | Testing edge case handling of the maximum permutation.                                        |
| How would you test this?                         | Testing for robustness (Duplicate elements, already sorted, reversed sorted, single element). |

