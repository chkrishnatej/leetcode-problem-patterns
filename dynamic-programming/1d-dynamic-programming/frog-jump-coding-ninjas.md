# Frog Jump - Coding Ninjas

{% embed url="https://www.codingninjas.com/codestudio/problems/frog-jump_3621012" %}

This is also similar to Climbing stairs problem. The constraints are same except here there is a cost /energy associated with each jump. And the stairs index start from 1.&#x20;

We need to find out what is the minimum amount of enery required for reaching the last step or $$n^{th}$$step.

## Intuition

We cannot go with greedy method as it optimizes for local maxima and not global maxima

<figure><img src="../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

#### Top Down

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

#### Bottom Up

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

![](<../../.gitbook/assets/image (82).png>)![](<../../.gitbook/assets/image (70).png>)

![](<../../.gitbook/assets/image (77).png>)![](<../../.gitbook/assets/image (74).png>)



{% tabs %}
{% tab title="Top Down" %}
```java
import java.util.*;
import java.io.*;


public class Solution {

    public static int frogJump(int n, int heights[]) {
        // n -> number of stairs to climb
        // Time Complexity: O(n), to traverse through each step and check min
        // Space Complexity: O(n), to store min values
        
        // Create an array to store the minimum cost of frog jumps for each position
        // Initialise the size of dp array to "n"
        int[] dp = new int[n];

        // Initialize the dp array with -1 to indicate that the minimum cost is not yet computed
        Arrays.fill(dp, -1);

        // Call the helper function to calculate the minimum cost
        helper(n - 1, heights, dp);

        // Return the minimum cost for the last position as "n-1"
        // because it starts from 1st step
        return dp[n - 1];
    }

    // Helper function to calculate the minimum cost of frog jumps
    private static int helper(int n, int[] heights, int[] dp) {
        // Base case: reached the starting position (cost is 0)
        if (n == 0) {
            return 0;
        }

        // If the minimum cost for the current position is already computed, return the precalculated value
        if (dp[n] != -1) {
            return dp[n];
        }

        // Calculate the cost of jumping to the current position from the previous positions
        int left = helper(n - 1, heights, dp) + Math.abs(heights[n] - heights[n - 1]);

        // Initialize the cost of jumping from the position two steps ago as a large value
        int right = Integer.MAX_VALUE;

        // If there is a position two steps ago, calculate the cost of jumping from that position
        if (n > 1) {
            right = helper(n - 2, heights, dp) + Math.abs(heights[n] - heights[n - 2]);
        }

        // Store the minimum cost in the dp array for future use
        return dp[n] = Math.min(left, right);
    }
}
```
{% endtab %}

{% tab title="Bottom Up" %}
```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int frogJump(int n, int heights[]) {
        // n -> number of stairs to climb
        // Time Complexity: O(n), to traverse through each step and check min
        // Space Complexity: O(n), to store min values
        
        // Create an array to store the minimum cost of frog jumps for each position
        int[] dp = new int[n];

        // Iterate through each position from left to right
        for (int i = 1; i < n; i++) {
            // Calculate the cost of jumping to the current position from the previous position (one step jump)
            int stepOneJump = dp[i - 1] + Math.abs(heights[i] - heights[i - 1]);

            // Initialize the cost of jumping from the position two steps ago as a large value
            int stepTwoJump = Integer.MAX_VALUE;

            // If there is a position two steps ago, calculate the cost of jumping from that position (two steps jump)
            if (i > 1) {
                stepTwoJump = dp[i - 2] + Math.abs(heights[i] - heights[i - 2]);
            }

            // Store the minimum cost of the two jump options in the dp array for the current position
            dp[i] = Math.min(stepOneJump, stepTwoJump);
        }

        // Return the minimum cost for the last position as "n-1"
        // because it starts at 1st step
        return dp[n - 1];
    }
}

```
{% endtab %}

{% tab title="BottomUp Space Optimisation" %}

{% endtab %}
{% endtabs %}
