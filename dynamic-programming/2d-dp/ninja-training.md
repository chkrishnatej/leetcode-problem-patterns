# Ninja Training

## Problem

{% embed url="https://www.codingninjas.com/codestudio/problems/ninja-s-training_3621003" %}

## Intuition

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

## Code

### Top Down - Memoization

{% hint style="info" %}
**Time Complexity:**`O(N*4*3)`

N -> Total Number of days

4 -> 4 states (3 activities + 1)

N\*4 -> For filling the 2D dp array with "-1"

3 -> Iterating it 3 times over all activities as we need to figure out all the possibilities

&#x20;

**Space Complexity:** `O(N)`

O(N\*4) -> DP array = O(N)

O(N) -> Recursion stack space = O(N)

Finally, O(N) + O(N) = O(2N) = O(N)
{% endhint %}



```java
import java.util.*;

public class Solution {

    /**
     * Calculates the maximum points that can be earned by doing a certain number of activities.
     *
     * @param day The current day.
     * @param last The last activity that was done.
     * @param points An array of points that represent the points that can be earned for each activity.
     * @param dp A 2D array that stores the maximum points that can be earned by doing a certain number of activities, given that the last activity was a certain activity.
     * @return The maximum points that can be earned by doing a certain number of activities.
     */
    static int helper(int day, int last, int[][] points, int[][] dp) {

        // Check if the value has already been calculated.
        if (dp[day][last] != -1) {
            return dp[day][last];
        }

        // Base case.
        if (day == 0) {
            // Find the maximum points that can be earned for each activity.
            int maxi = 0;
            for (int i = 0; i <= 2; i++) {
                if (i != last) {
                    maxi = Math.max(maxi, points[0][i]);
                }
            }

            // Return the maximum points.
            return dp[day][last] = maxi;
        }

        // Recursive case.
        // Find the maximum points that can be earned by doing the current activity, plus the maximum points that can be earned by doing the remaining activities, given that the last activity was the current activity.
        int maxi = points[day][last] + helper(day - 1, last, points, dp);

        // Find the maximum points that can be earned by doing the remaining activities, given that the last activity was a different activity.
        for (int i = 0; i <= 2; i++) {
            if (i != last) {
                maxi = Math.max(maxi, helper(day - 1, i, points, dp));
            }
        }

        // Return the maximum points.
        return dp[day][last] = maxi;
    }

    /**
     * Calculates the maximum points that can be earned by doing all of the activities.
     *
     * @param n The number of activities.
     * @param points An array of points that represent the points that can be earned for each activity.
     * @return The maximum points that can be earned by doing all of the activities.
     */
    static int ninjaTraining(int n, int[][] points) {

        // Create a 2D array of dp values.
        int[][] dp = new int[n][4];
        for (int[] row: dp)
            Arrays.fill(row, -1);

        // Recursively calculate the maximum points that can be earned by doing all of the activities.
        return helper(n - 1, 3, points, dp);
    }

    public static void main(String args[]) {

        // Create an array of points.
        int[][] points = {{10, 40, 70},
                          {20, 50, 80},
                          {30, 60, 90}};

        // Get the number of activities.
        int n = points.length;

        // Calculate the maximum points that can be earned by doing all of the activities.
        int maxPoints = ninjaTraining(n, points);

        // Print the maximum points.
        System.out.println("The maximum points that can be earned is " + maxPoints);
    }
}

```

### Bottom Up&#x20;

{% hint style="info" %}
**Time Complexity:**`O(N*4*3)`

N -> Total Number of days

4 -> 4 states (3 activities + 1)

N\*4 -> For filling the 2D dp array with "-1"

3 -> Iterating it 3 times over all activities as we need to figure out all the possibilities

&#x20;

**Space Complexity:** `O(N)`

O(N\*4) -> DP array = O(N)

O(N) -> Recursion stack space = O(N)

Finally, O(N) + O(N) = O(2N) = O(N)
{% endhint %}
