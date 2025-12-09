---
icon: ruler-triangle
---

# Right triangle pattern

### 📝 Note on Triangular Loop Structure

This pattern is a fundamental way to create triangular structures in programming, where each step (row) depends on the previous one. It's often used in visual patterns, mathematical series, and dynamic programming.

***

#### 🔑 Key Relationship: $$ $\text{Columns} = \text{Row Index} + 1$ $$

| **Component**        | **Code**                               | **Role in Pattern**                                                                                                                      |
| -------------------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Outer Loop (Rows)    | `for(int row=0;...;row++)`             | Controls the current line number.                                                                                                        |
| Inner Loop (Columns) | `for(int col=0; col < row + 1; col++)` | Controls how many times the star (`*`) is printed on the current line.                                                                   |
| Relationship         | `col < row + 1`                        | The number of columns is always one greater than the current zero-based row index. This is what creates the increasing triangular shape. |

***

#### 🌟 Simple Example ($$ $n=4$ $$)

If we set `int n = 4`, the code will execute as follows:

| **row** | **col condition (row+1)** | **Stars Printed** | **Output** |
| ------- | ------------------------- | ----------------- | ---------- |
| 0       | `col < 1`                 | 1                 | `*`        |
| 1       | `col < 2`                 | 2                 | `**`       |
| 2       | `col < 3`                 | 3                 | `***`      |
| 3       | `col < 4`                 | 4                 | `****`     |

***

### 💻 Usage in Data Structures and Algorithms (DSA)

The triangular loop structure is a powerful, concise tool in DSA, primarily used for tasks involving accumulated values, pairwise comparisons, and filling triangular matrices.

***

#### 1. Dynamic Programming (DP)

In many DP problems, the solution involves filling a 2D table where the value of a cell $$ $DP[i][j]$ $$ depends only on cells with indices $$ $k < i$ $$ or $$ $l < j$ $$. The triangular pattern is perfect for problems where the computation depends on a growing subproblem size.

* Example: Matrix Chain Multiplication (MCM)
  * Goal: Find the minimum number of scalar multiplications required to multiply a chain of matrices.
  * DP Table Structure: The DP table $$ $DP[i][j]$ $$ stores the minimum cost to multiply matrices from index $$ $i$ $$ to $$ $j$ $$. Since $$ $i$ $$ must be less than or equal to $$ $j$ $$, you only need the upper triangle of the matrix.
  *   Loop Implementation: The outer loop often controls the chain length (analogous to your `row`), and the inner loop iterates over the starting position of the chain. This effectively iterates over the upper triangular matrix:

      Java

      ```
      // Loop over chain length 'l' (analogous to row + 1)
      for (int l = 2; l < n; l++) { 
          // Loop over starting index 'i' (analogous to row)
          for (int i = 1; i <= n - l; i++) { 
              int j = i + l - 1; // End index (analogous to col)
              // DP calculation for DP[i][j]...
          }
      }
      ```
* Example: Longest Palindromic Subsequence/Substring (LPS)
  * The DP solution for LPS similarly relies on filling a table diagonally, where the calculation for a larger substring (length $$ $l$ $$) depends on the results of smaller substrings (length $$ $l-2$ $$), mirroring a triangular computation path.

***

#### 2. Graph Algorithms (Adjacency Matrices)

When dealing with a graph represented by an Adjacency Matrix $$ $A$ $$, where $$ $A[i][j] = A[j][i]$ $$ (undirected graph):

* Goal: Efficiently iterate over every unique edge without checking the same pair twice.
*   Loop Implementation: You use the triangular structure to check only the lower or upper half of the matrix (excluding the main diagonal where $$ $i=j$ $$):

    Java

    ```
    for (int i = 0; i < n; i++) {       // Outer loop (Row)
        for (int j = i + 1; j < n; j++) { // Inner loop (Column starts from i + 1)
            if (A[i][j] == 1) {
                // Process the unique edge (i, j)
            }
        }
    }
    ```

    This loop structure ensures that if you check the pair $$ $(i, j)$ $$, you will not check the pair $$ $(j, i)$ $$.

***

#### 3. Calculating Combinations / Pascal's Triangle

The loop is fundamentally used to generate Pascal's Triangle, a triangular array of binomial coefficients.

* Goal: Calculate $$ $\binom{n}{k}$ $$ for all $$ $0 \le k \le n$ $$.
*   Loop Implementation: The number of elements in each row of Pascal's Triangle increases by one, exactly matching the triangular loop structure:

    Java

    ```
    for (int row = 0; row < n; row++) {
        // 'col' iterates from 0 up to 'row', giving 'row + 1' elements
        for (int col = 0; col <= row; col++) { 
            // Calculate and print the binomial coefficient C(row, col)
        }
    }
    ```

    This provides the necessary structure to compute $$ $\binom{row}{col}$ $$ based on the values from $$ $\binom{row-1}{col-1}$ $$ and $$ $\binom{row-1}{col}$ $$.

<figure><img src="https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcQKzWsHc_yzC_2HRomk2KQxhUw33el3okpwrgqRwOELVZeAUNv-WA7oHILZnXLp_yfCn5V_R05HkLEW7pWHbkGszeK2ZiTEZCBmNn8Qt4TR4Ew15xQ" alt=""><figcaption></figcaption></figure>
