# Important Concepts

### _DFS vs BFS. Which would be more efficient? When would you prefer one over the other?_

If you were to get such a problem in an interview, it's very likely that the interviewer would proceed to ask a follow-up question such as this one. The DFS vs BFS is a vast topic of discussion. But one thing that's for sure (and helpful to know) is none is always better than the other. You would need to know the probable structure of the Tree/Graph that would be given as input (and, of course, what you need to find depending on the question ) to decide better which approach to prefer.

A **DFS** is easy to implement and generally has the advantage of being space-efficient, especially in a balanced / almost balanced Tree, and the space required would be `O(h)` (where `h` is the height of the tree) while we would require `O(2^h)` space complexity for **BFS** traversal, which could consume a huge amount of memory when the tree is balanced & for `h` is larger.

On the other hand, **BFS** would perform well if you don't need to search the entire depth of the tree or if the tree is skewed and there are only a few branches going very deep. In the worst case, the height of a tree `h` could be equal to `n` and if there are a huge number of nodes, DFS would consume huge amounts of memory & may lead to StackOverflow.

The DFS performed marginally better in this question, giving better space efficiency than BFS. Again, this depends on the structure of the trees used in the test cases.



