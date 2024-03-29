# Difference of subset sums is minimum

## Problem

{% embed url="https://www.codingninjas.com/studio/problems/partition-a-set-into-two-subsets-such-that-the-difference-of-subset-sums-is-minimum_842494" %}

## Intuition

In this problem, we need to find out what is the minimum difference between subset sums. All the numbers given are **non-negative** integers.

It is more like a modified extension of [416.-partition-equal-subset-sum.md](416.-partition-equal-subset-sum.md "mention") problem.

Here there is no target to be searched. As we ought to find the minimum subset sum difference, we take `totalSum` and find the partition for `totalSum`.

The core logic of finding the target subset is same.&#x20;

Once we fill in the DP table for finding the `totalSum` then we take the last row of DP and figure out what is the minimum difference between current sum and complement of it. We keep a track of the minimum difference.&#x20;

Here every number in last row of DP represents a sum. So to find minimum difference we need to take another subset sum, that can be found by `totalSum-currenSubsetSum` , keeping track of minimum of the difference is the solution.

## Time Complexity

n -> Number of elements in input array

```markup
O(n*totalSum) + O(n) + O(n)
```

`O(n*totalSum)` --> Number of loops

`O(n)` --> Finding totalSum

`O(n)` --> Finding the minimum subset sum difference

## Space Complexity

n -> Number of elements in input array

```
O(n)
```

It is space optimised approach

## Solution

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int minSubsetSumDifference(int[] arr, int n) {
        // This function finds the minimum difference between the sums of two subsets of a given set.

        // Initialize the total sum of the array.
        int totalSum = 0;
        for (int num : arr) {
            totalSum += num;
        }

        // Initialize the boolean array to store whether a subset with a given sum is possible.
        // The first element is always true since the sum of an empty subset is 0.
        boolean[] prev = new boolean[totalSum + 1];
        prev[0] = true;

        // If the first element of the array is less than or equal to the total sum, then it is possible to
        // have a subset with sum equal to the first element.
        if (arr[0] <= totalSum) {
            prev[arr[0]] = true;
        }

        // Iterate over the remaining elements of the array.
        for (int ind = 1; ind < n; ind++) {
            // Create a new boolean array to store whether a subset with a given sum is possible.
            boolean[] curr = new boolean[totalSum + 1];
            curr[0] = true;

            // Iterate over all possible sums.
            for (int target = 1; target <= totalSum; target++) {
                // If the current element is less than or equal to the target sum, then it is possible to
                // have a subset with sum equal to the target sum by including the current element.
                boolean notPick = prev[target];
                boolean pick = false;

                if (arr[ind] <= target) {
                    pick = prev[target - arr[ind]];
                }

                // Set the current element in the boolean array to true if it is possible to have a subset
                // with sum equal to the target sum by including or excluding the current element.
                curr[target] = pick || notPick;
            }

            // Update the previous boolean array with the current boolean array.
            prev = curr;
        }

        // Initialize the minimum difference between the sums of two subsets.
        int mini = Integer.MAX_VALUE;

        // Iterate over all possible sums that are less than or equal to half of the total sum.
        for (int i = 0; i <= totalSum / 2; i++) {
            // If it is possible to have a subset with sum equal to i, then update the minimum difference.
            if (prev[i] == true) {
                int diff = Math.abs(i - (totalSum - i));
                mini = Math.min(diff, mini);
            }
        }

        // Return the minimum difference between the sums of two subsets.
        return mini;
    }
}

```
