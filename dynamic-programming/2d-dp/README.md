# 2D - DP

To understand if we have to apply pre computation programming or not, we need to run through an example. Take a small example and try greedy method on it and check. If it is not giving the required answer in the given constraint we need to go for DP.

Basically in DP, we need to explore all the paths available. So we try **top-down** approach, i.e. first we convert it into a recurrence problem.&#x20;

Draw a recurrence tree and see what are the function calls which are being made. Then you will understand what **overlapping subproblems** are present. To solve the overlapping sub problems, we will introduce a cache called `dp` array.&#x20;

In `dp` array we store the computation of required parameter under the given constraint, when we encounter the same problem we pick it from the `dp` array.&#x20;

To convert it into **bottom-up** approach, we need to check the edge cases and figure out the initial pre computation like what should be on the "zeroth" index. And use the same recurrence relation that we have figured it out when using the **recursion** approach. Build the tabulation till the end of the input array.&#x20;

To see if we can space optimize it, we need to check what possible pre computed valuables are required, generally we might need 4 or 5 of them. So we can store them in variables instead of allocating a `dp` array. This reduces the space complexity
