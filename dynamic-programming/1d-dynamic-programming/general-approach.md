# General Approach

## Top Down - Memoization

```java
public class Solution {
    public static int frogJump(int n, int heights[], int k) {
        // Create an array to store the minimum cost of frog jumps for each position
        int[] dp = new int[n];
        Arrays.fill(dp, -1);

        // Call the helper function to calculate the minimum cost for the last position
        helper(n - 1, k, heights, dp);

        // Return the minimum cost for the last position
        return dp[n - 1];
    }

    private static int helper(int n, int k, int[] heights, int[] dp) {
        // Base case: if the current position is the starting position (index 0), return 0
        if (n == 0) {
            return 0;
        }

        // If the minimum cost for the current position has already been calculated, return it
        if (dp[n] != -1) {
            return dp[n];
        }

        // Initialize the minimum steps variable to a large value
        int minSteps = Integer.MAX_VALUE;

        // Iterate through all possible jumps from 1 to k steps
        for (int j = 1; j <= k; j++) {
            // Check if jumping j steps from the current position is within the array bounds
            if (n - j >= 0) {
                // Calculate the cost of jumping j steps from the current position
                int jump = helper(n - j, k, heights, dp) + Math.abs(heights[n] - heights[n - j]);

                // Update the minimum steps with the smaller value between the current jump and the minimum steps so far
                minSteps = Math.min(minSteps, jump);
            }
        }

        // Store the minimum steps in the dp array for the current position
        return dp[n] = minSteps;
    }
}
```

## Bottom Up - Tabulation

```java
public class Solution {
    public static int frogJump(int n, int heights[], int k) {
        // Create an array to store the minimum cost of frog jumps for each position
        int[] dp = new int[n];
        
        // Initialize the minimum cost for the starting position (index 0)
        dp[0] = 0;
        
        // Iterate through each position and calculate the minimum cost of frog jumps
        for (int i = 1; i < n; i++) {
            // Initialize the minimum steps variable to a large value
            int minSteps = Integer.MAX_VALUE;

            // Iterate through all possible jumps from 1 to k steps
            for (int j = 1; j <= k; j++) {
                // Check if jumping j steps from the current position is within the array bounds
                if (i - j >= 0) {
                    // Calculate the cost of jumping j steps from the current position
                    int jump = dp[i - j] + Math.abs(heights[i] - heights[i - j]);

                    // Update the minimum steps with the smaller value between the current jump and the minimum steps so far
                    minSteps = Math.min(minSteps, jump);
                }
            }
            // Store the minimum steps in the dp array for the current position
            dp[i] = minSteps;
        }

        // Return the minimum cost for the last position
        return dp[n - 1];
    }
}

```
