# Java Code

## Problem Statement

This code solves your "**Final Boss**" problem (10 cookies, 4 kids, each $$\ge 1$$, Kid A $$\le 2$$, Kid B $$\le 2$$).

```java
public class StarsAndBars {

    // Efficiently calculate nCr to avoid overflow
    public static long combinations(int n, int r) {
        // n ➔ Total selection set
        // r ➔ No. of items to be selected from total sets
    
        // r < 0 ➔  Invalid number of selections 
        // r > 0 ➔  Eg: Picking up 5 slots from 3 available slots 
        if (r < 0 || r > n) return 0;
        // r == 0 ➔ Choosing no slot (empty set)
        // r == n ➔ Choosing all available slots
        if (r == 0 || r == n) return 1;
        if (r > n / 2) r = n - r; // Optimize: nC3 is easier than nC7
        
        // Using `long` for memory awareness, makes sure the result can be fit in long
        // If even long is not enough, use BigInteger
        long result = 1;
        // Iterate from 1 to the r (number of selected set)
        for (int i = 1; i <= r; i++) {
            result = result * (n - i + 1) / i;
        }
        return result;
    }

    public static void main(String[] args) {
        int totalCookies = 10;
        int k = 4;

        // 1. Global Pre-allocation (each kid gets at least 1) [cite: 54, 68]
        int n = totalCookies - k; // 10 - 4 = 6

        // 2. Total Baseline: (n + k - 1) choose (k - 1) [cite: 38, 46, 61]
        long totalWays = combinations(n + k - 1, k - 1); // 9C3 = 84

        // 3. Illegal States (Kid A and Kid B have limit of 2)
        // Since they already have 1, the violation is x >= 2 additional cookies. [cite: 71, 81]
        int violationShift = 2; 

        // Illegal A: Pre-allocate violation shift to Kid A [cite: 82, 83]
        long illegalA = combinations((n - violationShift) + k - 1, k - 1); // 7C3 = 35

        // Illegal B: Pre-allocate violation shift to Kid B
        long illegalB = combinations((n - violationShift) + k - 1, k - 1); // 7C3 = 35

        // 4. Overlap: Both A and B violate their limits 
        // Pre-allocate violation to both: n - 2 - 2
        long overlap = combinations((n - 2 * violationShift) + k - 1, k - 1); // 5C3 = 10

        // 5. Final Result: Total - (A + B) + Overlap
        long result = totalWays - (illegalA + illegalB) + overlap;

        System.out.println("Valid combinations: " + result); // Output: 24
    }
}
```

***

#### Time Complexity

The time complexity of the `combinations(n, r)` function is $$O(r)$$.

* Loop Execution: The function executes a single `for` loop that runs from $$1$$ to $$r$$.
* Efficiency: By using the symmetry property ($$\binom{n}{r} = \binom{n}{n-r}$$), the code ensures that $$r$$ is always less than or equal to $$n/2$$. This means in a case like $$\binom{100}{98}$$, the loop only runs 2 times instead of 98.
* Operations: Inside the loop, only basic arithmetic operations (multiplication and division) occur, which are $$O(1)$$.

#### Space Complexity

The space complexity is $$O(1)$$.

* Fixed Memory: The function only stores a few primitive variables (`result`, `i`, `n`, `r`).
*   No Recursion: Unlike dynamic programming approaches for combinations that require a table ($$O(n \times r)$$) or recursive approaches that use the call stack, this iterative method uses a constant amount of extra space regardless of the input size.\
    <br>

    | **Phase**            | **Time Complexity** | **Space Complexity** |
    | -------------------- | ------------------- | -------------------- |
    | `combinations(n, r)` | $$O(r)$$            | $$O(1)$$             |
    | Complete PIE Logic   | $$O(2^m \times r)$$ | $$O(1)$$             |
