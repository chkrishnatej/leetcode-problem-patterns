# Walls and Gates

## Problem

{% embed url="https://leetcode.com/problems/walls-and-gates/description/" %}

## Approach

The problem is given in the form of `int[][]` but it can be modelled to form of graph. When we model it in the form of graph, we can apply BFS which optimally finds the solution in least amount of time.

* We create a queue
* Add all existing gates to the queue
* Process the queue until empty
  * Poll the cell from queue
  * Explore up, down, left and right sides from the polled cell
  * Make sure the coordinates which were generated from polled cell, are valid. Valid means:
    * Coordinates value should not be less than zero
    * Coordinates value should not be greater than equal to size of the graph
    * Should not be a wall
  * Once we make sure, it is a valid cell to be processed, we increment the new cell value by 1 with value from polled cell
  * Add the new cell to queue for further processing

We add the new cell too queue because, they are part of solution and we have not yet processed the cell completely. Using BFS makes sure, we process each cell only once.

## Complexity

> V -> Vertices (cells)
>
> E -> Edges (connection between the cells)

The walls are not vertices in the graph, so they are not processed by the BFS algorithm.

### Time Complexity:

{% hint style="info" %}
**O(|V|+|E|)**

It follows standard BFS algorithm. We explore all the cells before we process cells which are not walls. And during the processing of cells in the queue, we need to explore their edges i.e. up, down, right and left.
{% endhint %}

### Space Complexity

{% hint style="success" %}
O(|V|)

Even though we are only processing cells which are not walls. This is because the algorithm still needs to store a queue of size |V|. The queue is used to store the vertices that have not yet been visited.
{% endhint %}

## Code

```java
class Solution {
    public void wallsAndGates(int[][] rooms) {
        //The problem can be modelled into a graph and BFS can be applied
        // V -> Vertices of graph (cells)
        // E -> Edges (connection between the cell)
        // Time Complexity: O(|V|+|E|)
        // Space Complexity: O(|V|)

        int m = rooms.length;
        int n = m == 0 ? 0 : rooms[0].length;

        // Exploring up, down, left and right directions
        int[][] dirs = {{-1,0}, {1,0}, {0,1}, {0,-1}};

        // Queue to process the nodes in order
        Deque<int[]> queue = new ArrayDeque<>();

        // Add all gates to the queue
        for (int i=0; i<m; i++) {
            for (int j=0; j<n; j++) {
                if (rooms[i][j] == 0) { // Room value 0 means it is a gate
                    // Add the gates to queue to be processed
                    queue.offer(new int[] {i,j}); 
                }
            }
        }
    
        // Update distance from gates
        while (!queue.isEmpty()) {
            // Poll the gate
            int[] curPos = queue.poll();
            for (int[] dir: dirs) { // Check the four directons
                int X = curPos[0] + dir[0]; // Gets the X coordinate of next possible cell
                int Y = curPos[1] + dir[1]; // Gets the y coordinate of next possible cell
                // Check edge cases like underflow and overflow. Alsoo checks if
                // given cell is wall or not by checking if its infiinite value
                // If any of below conditions true, then do nothing and come
                // out of iteration
                if (X < 0 || Y < 0 || X >= m || Y >= n || rooms[X][Y] != Integer.MAX_VALUE) {
                    continue;
                } 
                // If it is a valid cell, then increment the value of original
                // cell by 1
                rooms[X][Y] = rooms[curPos[0]][curPos[1]]+1;
                // Add the new coordinates to the queue for processing
                /* BFS algorithm works by repeatedly adding the neighbors of the 
                current cell to the queue. This ensures that we explore all of the 
                cells in the grid in a breadth-first manner.
                By pushing them to the queue, we are ensuring that they will be 
                explored in the next iteration of the BFS algorithm. This ensures 
                that we will eventually explore all of the cells in the grid.
                */ 
                queue.offer(new int[] {X, Y});
            }
        }
    }
}
```
