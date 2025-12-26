---
icon: star
---

# Stars and Bars

#### Complete Framework: Stars and Bars

**1. Core Theory**

Stars and Bars is a combinatorial technique used to find the number of ways to distribute $$n$$ identical items into $$k$$ distinct bins.

In this model:

* Stars ($$\star$$): Represent the identical items.
* Bars ($$|$$): Represent the dividers that create the bins. To create $$k$$ bins, you need $$k-1$$ bars.

The fundamental principle is Bijective Mapping: Every unique arrangement of stars and bars corresponds to exactly one way to distribute the items.

***

**2. The Two Primary Cases**

| **Feature** | **Case 1: Non-Negative Integers**     | **Case 2: Positive Integers**                |
| ----------- | ------------------------------------- | -------------------------------------------- |
| Condition   | $$x_i \ge 0$$ (Bins can be empty)     | $$x_i \ge 1$$ (Every bin gets $$ $\ge 1$ $$) |
| Logic       | Arrange $$n$$ stars and $$k-1$$ bars. | Pre-place 1 star in each of the $$k$$ bins.  |
| Formula     | $$\binom{n + k - 1}{k - 1}$$          | $$\binom{n - 1}{k - 1}$$                     |
| Example     | Distributing cookies to kids.         | Partitioning a sum into variables $$\ge 1$$. |

***

**3. Mathematical Derivation (Case 1)**

To distribute $$n$$ stars into $$k$$ bins:

1. We have $$n$$ items and $$k-1$$ dividers.
2. Total positions to fill = $$n + (k-1)$$.
3. We must choose $$k-1$$ of these positions to place our bars (the rest automatically become stars).
4. Result: $$\binom{n+k-1}{k-1}$$.

***

**4. Handling Constraints (L5 Logic)**

**A. Lower Bound Constraints ("At Least** $$ $M$ $$**")**

If a bin $$x_i$$ must have at least $$M$$ items:

1. Induce Shift: Pre-allocate $$M$$ items to that bin.
2. Adjust $$n$$: $$n_{new} = n_{total} - M$$.
3. Calculate: Use the standard formula with the updated $$n$$.

**B. Upper Bound Constraints ("At Most** $$M$$**")**

If a bin $$x_i$$ must have at most $$M$$ items, use the Principle of Inclusion-Exclusion (PIE):

1. Calculate Total: Find combinations without the upper limit.
2. Calculate Illegal State: Force the bin to violate the limit ($$x_i \ge M + 1$$).
3. Subtract: $$\text{Valid} = \text{Total} - \text{Illegal}$$.

***

**5. Application: Lexicographical Sorting (LeetCode 1641)**

* **Identity**: Sorted strings are a special case of Stars and Bars.
* **The Principle**: Since the order of characters is fixed by the "sorted" rule, the string is defined entirely by the frequency of each character.
* **Mapping**: \* $$n$$ (Stars) = Length of the string.
  * $$k$$ (Bins) = Number of available characters (e.g., 5 vowels).
  * **Formula**: $$\binom{n+5-1}{5-1} = \binom{n+4}{4}$$.

***

**6. Java Implementation (Computational Efficiency)**

```java
public long combinations(int n, int r) {
    if (r < 0 || r > n) return 0;
    if (r == 0 || r == n) return 1;
    if (r > n / 2) r = n - r; // Symmetry property

    long result = 1;
    for (int i = 1; i <= r; i++) {
        result = result * (n - i + 1) / i;
    }
    return result;
}
```

**Complexity:**

* Time: $$O(r)$$, where $$r$$ is the number of bars.
* Space: $$O(1)$$, using iterative calculation to prevent stack overflow and minimize memory.

***

#### Examination Problem: Multi-Constraint PIE

**Scenario:** Find the number of non-negative integer solutions to:

$$
x_1 + x_2 + x_3 = 15
$$

**Constraints:**

1. Lower Bound: $$x_1 \ge 2$$, $$x_2 \ge 1$$, $$x_3 \ge 0$$.
2. Upper Bound: $$x_1 \le 5$$.

***

#### Phase 1: Total Baseline (Lower Bounds)

Before addressing the "at most" limit, we must satisfy the "at least" requirements.

* Initial Stars ($$n$$): 15
* Pre-allocation: $$x_1$$ takes 2, $$x_2$$ takes 1.
* Shifted Stars ($$n'$$): $$15 - 2 - 1 = 12$$.
* Bins ($$k$$): 3 variables.
* Bars ($$r$$): $$3 - 1 = 2$$.

**Total Baseline Calculation:**

$$
\binom{n' + k - 1}{k - 1} = \binom{12 + 3 - 1}{3 - 1} = \binom{14}{2} = \frac{14 \times 13}{2} = 91
$$

***

#### Phase 2: Illegal State (Upper Bound Violation)

The constraint is $$x_1 \le 5$$. We must find how many ways $$x_1$$ can be 6 or greater.

* Current State: $$x_1$$ already has 2 stars from pre-allocation.
* Target for Violation: $$x_1$$ needs to reach a total of 6 stars.
* Induced Shift: To go from 2 stars to 6 stars, $$x_1$$ needs 4 more stars from our remaining pool of 12.
* Remaining Stars ($$n''$$): $$12 - 4 = 8$$.

**Illegal State Calculation:**

$$
\binom{n'' + k - 1}{k - 1} = \binom{8 + 3 - 1}{3 - 1} = \binom{10}{2} = \frac{10 \times 9}{2} = 45
$$

***

#### Phase 3: Final Result

Apply the Principle of Inclusion-Exclusion (PIE):

$$$$$
\text{Valid Solutions} = \text{Total Baseline} - \text{Illegal State}$$$$\text{Valid Solutions} = 91 - 45 = 46
$$$$$

***

#### Summary Table for Review

| **State**                 | **n (Stars)** | **r (Bars)** | **Result** |
| ------------------------- | ------------- | ------------ | ---------- |
| Baseline                  | 12            | 2            | 91         |
| Illegal ($$ $x_1 > 5$ $$) | 8             | 2            | 45         |
| Final                     | -             | -            | 46         |

***

Python

```python
import math

def nCr(n, r):
    if r < 0 or r > n:
        return 0
    return math.comb(n, r)

# Total
n_prime = 16 # 20 - 4
total = nCr(n_prime + 4 - 1, 4 - 1)

# Violation A: x1 >= 5 (needs 4 more stars)
v_a = nCr(16 - 4 + 3, 3)

# Violation B: x2 >= 6 (needs 5 more stars)
v_b = nCr(16 - 5 + 3, 3)

# Violation A and B: x1 >= 5 and x2 >= 6 (needs 4 + 5 = 9 more stars)
v_ab = nCr(16 - 9 + 3, 3)

final = total - (v_a + v_b - v_ab)

print(f"{total=}")
print(f"{v_a=}")
print(f"{v_b=}")
print(f"{v_ab=}")
print(f"{final=}")


```

Code output

```
total=969
v_a=455
v_b=364
v_ab=120
final=270
```

#### Problem Statement: Multi-Constraint PIE

Find the number of non-negative integer solutions to:\
$$x_1 + x_2 + x_3 + x_4 = 20$$

**Constraints:**

1. Lower Bounds: $$x_i \ge 1$$ for all $$i \in \{1, 2, 3, 4\}$$.
2. Upper Bounds: $$x_1 \le 4$$ and $$x_2 \le 5$$.

***

#### Derivation

**1. Baseline Calculation**

Apply global lower bounds $$x_i \ge 1$$:

* $$n_{stars} = 20 - 4 = 16$$
* $$k_{bins} = 4$$
* $$r_{bars} = 3$$
* $$S = \binom{16+4-1}{4-1} = \binom{19}{3} = 969$$

**2. Principle of Inclusion-Exclusion (PIE)**

Let $A$ be the set of solutions where $$x_1$$ violates its upper bound ( $$x_1 \ge 5$$).

Let $$B$$ be the set of solutions where $$x_2$$ violates its upper bound $$(x_2 \ge 6)$$.

Set $$A$$ (Violation of $$x_1$$):

$$x_1$$ already has 1 star. To reach $$\ge 5$$, we must shift 4 additional stars.

* $$n_A = 16 - 4 = 12$$
* $$|A| = \binom{12+3}{3} = \binom{15}{3} = 455$$

Set $$B$$ (Violation of $$x_2$$):

$$x_2$$ already has 1 star. To reach $$\ge 6$$, we must shift 5 additional stars.

* $$n_B = 16 - 5 = 11$$
* $$|B| = \binom{11+3}{3} = \binom{14}{3} = 364$$

Set $$A_CI A \cap BA\capB$$ A \cap B (Simultaneous Violation):

Shift 4 stars for $$x_1$$ and 5 stars for $$x_2$$

* $$n_{A \cap B} = 16 - (4 + 5) = 7$$
* $$|A \cap B| = \binom{7+3}{3} = \binom{10}{3} = 120$$

**3. Final Calculation**

Valid Solutions = Total - (Violations)

$$
\text{Valid} = S - (|A| + |B| - |A \cap B|)\text
$$

***

#### Java Implementation

This implementation uses the PIE framework to handle arbitrary upper-bound constraints.

```java
public class StarsAndBarsPIE {
    public static void main(String[] args) {
        int n = 20;
        int k = 4;
        // Adjusted n after lower bounds x_i >= 1
        int adjustedN = n - k; 
        
        // Limits after lower bounds: x1 <= 3 (4-1), x2 <= 4 (5-1)
        int[] limits = {3, 4}; 

        System.out.println(solve(adjustedN, k, limits));
    }

    public static long solve(int n, int k, int[] limits) {
        long totalValid = 0;
        int numLimits = limits.length;
        
        // Iterate through all 2^numLimits subsets of constraints to apply PIE
        for (int i = 0; i < (1 << numLimits); i++) {
            int currentN = n;
            int bitsSet = 0;
            
            for (int j = 0; j < numLimits; j++) {
                if (((i >> j) & 1) == 1) {
                    // To violate x_j <= limit_j, force x_j >= limit_j + 1
                    currentN -= (limits[j] + 1);
                    bitsSet++;
                }
            }
            
            long combinations = nCr(currentN + k - 1, k - 1);
            if (bitsSet % 2 == 1) {
                totalValid -= combinations;
            } else {
                totalValid += combinations;
            }
        }
        return totalValid;
    }

    private static long nCr(int n, int r) {
        if (r < 0 || r > n) return 0;
        if (r == 0 || r == n) return 1;
        if (r > n / 2) r = n - r;
        
        long res = 1;
        for (int i = 1; i <= r; i++) {
            res = res * (n - i + 1) / i;
        }
        return res;
    }
}
```
