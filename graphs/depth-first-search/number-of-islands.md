# Number of Islands

## Problem

[https://leetcode.com/problems/number-of-islands/](https://leetcode.com/problems/number-of-islands/)

## Concept

The idea is to find the total number of connected components. Although we can use Union Find, it can be solved with DFS.&#x20;

## Algorithm

1. Initialize the result variable to 0.
2. Iterate over each cell in the grid.
3. If the cell is land, then:
   * Recursively call the dfs function on the cell.
   * Increment the result variable by 1.
4. Return the result variable.

The dfs function works as follows:

1. Check if the current cell is out of bounds or if it is already visited. If it is, then return.
2. Mark the current cell as visited.
3. Recursively call the dfs function on the four neighbors of the current cell.

## Code

```java
class Solution {
    public int numIslands(char[][] grid) {
        // V --> Vertices (nodes)
        // E --> Edges (connections)
        // Time complexity: O(V+E) --> Standard DFS time complexity
        // Space complexity: O(V)
        // Edge case
        if(grid==null)
            return 0;
        
        int result = 0;
        
        // The input is given as adjacency matrix itself. 
        // Hence, no pre processing is required
        
        // Traverse through each node in the grid
        for(int i=0;i<grid.length;i++) {
            for(int j=0;j<grid[0].length;j++) {
                if(grid[i][j]=='1') { // When land is encountered
                    dfs(i, j, grid); // Explore if there exists island
                    result++; // Increment the islandss count
                }
            }
        }
        return result;
    }
    
    private void dfs(int i, int j, char[][] grid) {
        // Boundary cases
        if(i<0 || i>=grid.length || j<0 || j>=grid[0].length || grid[i][j]=='0')
            return;
        
        grid[i][j] = '0'; // Marking the node as visited
        
        dfs(i-1, j, grid); // Check up
        dfs(i+1, j, grid); // Check down
        dfs(i, j-1, grid); // Check left
        dfs(i, j+1, grid); // Check right
    }
}
```
