# Number of Islands II

## Problem

{% embed url="https://leetcode.com/problems/number-of-islands-ii/description/" %}

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

![](<../../.gitbook/assets/image (37).png>)![](<../../.gitbook/assets/image (15).png>)



## Concept

This is a classic problem which uses disjoint set. In this problem, we will explore adjacent nodes when trying to convert sea to land. But we might run into a case where we do not know to which island group the current island belongs to.

What the question is looking for is number islands which are not overlapping each other.

## Code



{% tabs %}
{% tab title="Union By Rank" %}
The time and space complexity for this algorithm is **`O(m*n)`**

```java
class DisjointSet {
    List<Integer> parent = new ArrayList<>();
    List<Integer> rank = new ArrayList<>();

    public DisjointSet(int n) {
        for(int i=0;i<n;i++) {
            parent.add(i);
            rank.add(10);
        }
    }

    public int findParent(int node) {
        if(node == parent.get(node)) {
            return node;
        }
        int parentNode = findParent(parent.get(node));
        parent.set(node, parentNode);
        return parent.get(node);
    }

    public void unionByRank(int x, int y) {
        int parentX = findParent(x);
        int parentY = findParent(y);

        if(parentX == parentY) {
            return;
        }

        int rankX = rank.get(parentX), rankY = rank.get(parentY);

        if(rankX < rankY) {
            parent.set(rankX, rankY);
        } else if(rankY < rankX) {
            parent.set(rankY, rankX);
        } else {
            parent.set(parentX, parentY);
            rank.set(parentX, rankX+1);
        }
        return;
    }
}
class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        // Time Complexity: O(m*n)
        // Space Complexity: O(m*n)
    
  // `dirs` is a 2D array of directions. Each row of `dirs` represents a
  // direction, where `dirs[0]` represents moving up, `dirs[1]` represents
  // moving down, `dirs[2]` represents moving left, and `dirs[3]` represents
  // moving right.
  int[][] dirs = {{0, -1}, {0, 1}, {-1, 0}, {1, 0}};

  // `visited` is a boolean array that indicates whether a cell has been
  // visited.
  boolean[] visited = new boolean[m * n];

  // `result` is a list of integers that will store the number of islands
  // in each row.
  List<Integer> result = new ArrayList<>();

  // `count` is a variable that keeps track of the number of islands.
  int count = 0;

  // `ds` is a disjoint set data structure that will be used to keep track
  // of the connected components of the grid.
  DisjointSet ds = new DisjointSet(m * n);

  // Iterate over each position in the `positions` array.
  for (int[] position : positions) {
    // Get the x-coordinate and y-coordinate of the current position.
    int X = position[0];
    int Y = position[1];

    // Get the index of the current position in the grid.
    int V = X * n + Y;

    // If the current position has already been visited, then skip it.
    if (visited[V]) {
      continue;
    }

    // Mark the current position as visited.
    visited[V] = true;

    // Increment the count of islands by 1.
    count++;

    // Iterate over each direction in the `dirs` array.
    for (int[] dir : dirs) {
      // Get the x-coordinate and y-coordinate of the adjacent position.
      int adjX = X + dir[0];
      int adjY = Y + dir[1];

      // Get the index of the adjacent position in the grid.
      int adjV = adjX * n + adjY;

      // If the adjacent position is out of bounds or if it has already been
      // visited, then skip it.
      if (adjX < 0 || adjX >= m || adjY < 0 || adjY >= n || visited[adjV]) {
        continue;
      }

      // If the parent of the current position is not equal to the parent of
      // the adjacent position, then merge the two connected components and
      // decrement the count of islands by 1.
      if (ds.findParent(V) != ds.findParent(adjV)) {
        count--;
        ds.unionByRank(V, adjV);
      }
    }

    // Add the count of islands to the `result` list.
    result.add(count);
  }

  // Return the `result` list.
  return result;
}

}
```
{% endtab %}

{% tab title="Union By Size" %}
The time and space complexity for this algorithm is **`O(m*n)`**

```java
class DisjointSet {
    List<Integer> parent = new ArrayList<>();
    List<Integer> size = new ArrayList<>();

    public DisjointSet(int n) {
        for(int i=0;i<n;i++) {
            parent.add(i);
            size.add(1);
        }
    }

    public int findParent(int node) {
        if(node == parent.get(node)) {
            return node;
        }
        int parentNode = findParent(parent.get(node));
        parent.set(node, parentNode);
        return parent.get(node);
    }

    public void unionBySize(int x, int y) {
        int parentX = findParent(x);
        int parentY = findParent(y);

        if(parentX == parentY) {
            return;
        }

        int sizeX = size.get(parentX), sizeY = size.get(parentY);

        if(sizeX < sizeY) {
            parent.set(parentX, parentY);
            size.set(parentY, sizeX + sizeY);
        } else {
            parent.set(parentY, parentX);
            size.set(parentX, sizeX+sizeY);
        }
       
        return;
    }
}
class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        // Time Complexity: O(m*n)
        // Space Complexity: O(m*n)
    
      // `dirs` is a 2D array of directions. Each row of `dirs` represents a
      // direction, where `dirs[0]` represents moving up, `dirs[1]` represents
      // moving down, `dirs[2]` represents moving left, and `dirs[3]` represents
      // moving right.
      int[][] dirs = {{0, -1}, {0, 1}, {-1, 0}, {1, 0}};

      // `visited` is a boolean array that indicates whether a cell has been
      // visited.
      boolean[] visited = new boolean[m * n];

      // `result` is a list of integers that will store the number of islands
      // in each row.
      List<Integer> result = new ArrayList<>();

      // `count` is a variable that keeps track of the number of islands.
      int count = 0;

      // `ds` is a disjoint set data structure that will be used to keep track
      // of the connected components of the grid.
      DisjointSet ds = new DisjointSet(m * n);

      // Iterate over each position in the `positions` array.
      for (int[] position : positions) {
          // Get the x-coordinate and y-coordinate of the current position.
          int X = position[0];
          int Y = position[1];

          // Get the index of the current position in the grid.
          int V = X * n + Y;

          // If the current position has already been visited, then skip it.
          if (visited[V]) {
            continue;
          }

          // Mark the current position as visited.
          visited[V] = true;

          // Increment the count of islands by 1.
          count++;
          
         // Iterate over each direction in the `dirs` array.
        for (int[] dir : dirs) {
          // Get the x-coordinate and y-coordinate of the adjacent position.
          int adjX = X + dir[0];
          int adjY = Y + dir[1];

          // Get the index of the adjacent position in the grid.
          int adjV = adjX * n + adjY;

          // If the adjacent position is out of bounds or if it has already been
          // visited, then skip it.
          if (adjX < 0 || adjX >= m || adjY < 0 || adjY >= n || visited[adjV]) {
            continue;
          }

          // If the parent of the current position is not equal to the parent of
          // the adjacent position, then merge the two connected components and
          // decrement the count of islands by 1.
          if (ds.findParent(V) != ds.findParent(adjV)) {
            count--;
            ds.unionBySize(V, adjV);
          }
        }

        // Add the count of islands to the `result` list.
        result.add(count);
    }

      // Return the `result` list.
      return result;
  }
}
```
{% endtab %}
{% endtabs %}
