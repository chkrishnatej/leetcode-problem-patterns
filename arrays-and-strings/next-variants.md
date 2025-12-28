# Next Variants

#### 1. Next Greater Element Variants

These problems focus on monotonic stack applications, circularity, and range queries.

* [LeetCode 496](https://leetcode.com/problems/next-greater-element-i/description/) ➔ Next Greater Element I: The basic introduction using a HashMap and a Monotonic Stack.
* [LeetCode 503](https://leetcode.com/problems/next-greater-element-ii/) ➔  Next Greater Element II: Introduces Circular Arrays. Requires the "double iteration" or "modulo" technique.
* [LeetCode 556](https://leetcode.com/problems/next-greater-element-iii/) ➔ Next Greater Element III: A direct bridge to the Next Permutation algorithm. It asks for the next greater integer using the same digits.
* [LeetCode 739 ](https://leetcode.com/problems/daily-temperatures/)➔  Daily Temperatures: A practical application. Instead of the value, you return the distance (indices) to the next greater element.
* [LeetCode 84](https://leetcode.com/problems/largest-rectangle-in-histogram/) ➔ Largest Rectangle in Histogram: An L5 staple. Uses NGE logic (specifically Previous/Next Smaller Element) to find the width of the rectangle a bar can sustain.

***

#### 2. Next Permutation Variants

These problems test the lexicographical logic, ranking, and inversions.

* [LeetCode 31 ](https://leetcode.com/problems/next-permutation/)➔  Next Permutation: The core problem discussed. Implement the pivot-swap-reverse logic.
* [LeetCode 60](https://leetcode.com/problems/permutation-sequence/) ➔  Permutation Sequence: Finds the K-th Permutation. Requires the Factoradic (factorial number system) approach rather than iterative calls.
* [LeetCode 267](https://leetcode.com/problems/palindrome-permutation-ii/) ➔  Palindrome Permutation II: Combines permutation logic with backtracking. You must generate all unique palindromic permutations.
* [LintCode 51](https://www.lintcode.com/problem/51/) ➔ Previous Permutation: Inverts the logic to find the largest permutation smaller than the current one. Excellent for testing symmetry in your logic.

***

#### 3. Advanced / Google-Style Twists

These problems move toward tree structures or complex constraints often seen in L5 interviews.

* [LeetCode 98](https://leetcode.com/problems/validate-binary-search-tree/) ➔  Validate Binary Search Tree: While seemingly unrelated, the NGE in a BST is its In-order Successor. This tests the connection between linear arrays and tree topologies.
* [LeetCode 316](https://leetcode.com/problems/remove-duplicate-letters/) ➔  Remove Duplicate Letters: Uses a monotonic stack to maintain Lexicographical Order while satisfying the constraint that every character must appear exactly once.
* [LeetCode 402](https://leetcode.com/problems/remove-k-digits/) ➔  Remove K Digits: Uses a monotonic stack to find the smallest possible number. This tests the "minimal lexicographical decrease" logic.
* [LeetCode 1856](https://leetcode.com/problems/maximum-subarray-min-product/) ➔  Maximum Subarray Min-Product: A "harder" variant of the Histogram problem. Uses NGE logic to define the range where an element is the minimum.

***

#### Summary Table for L5 Study Plan

| **Category**    | **Must-Solve Problem**                                                                              | **Core Skill Tested**                         |
| --------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| Monotonic Stack | [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) | Visibility boundaries / $$O(n)$$ optimization |
| Lexicographical | [31. Next Permutation](https://leetcode.com/problems/next-permutation/)                             | Suffix properties / Greedy minimal change     |
| Combinatorics   | [60. Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)                     | Factorial number system / Math efficiency     |
| Constraints     | [402. Remove K Digits](https://leetcode.com/problems/remove-k-digits/)                              | Monotonic logic with greedy pruning           |

