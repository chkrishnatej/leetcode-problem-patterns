# Depth First Search

## Adjacency List

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

In Java it is represented by:

```java
Map<Integer, List<Integer>> map = new HashMap<>();
```

The key is the node and value is the nodes connected to it



### Time and Space Complexity

| Complexity       | Best Case | Worst Case |
| ---------------- | --------- | ---------- |
| Time Complexity  | O(V + E)  | O(V + E)   |
| Space Complexity | O(V)      | O(V)       |

### Code&#x20;

```java
import java.util.*;

class Solution {
    public void dfs(List<List<Integer>> graph, int start) {
        // Initialize a boolean array to keep track of visited nodes
        boolean[] visited = new boolean[graph.size()]; 
        // Call the helper function to perform DFS
        dfsHelper(graph, start, visited); 
    }
    
    private void dfsHelper(List<List<Integer>> graph, int node, boolean[] visited) {
         // Mark the current node as visited
        visited[node] = true;
        
        // Do something with the visited node (e.g., print its value)
        System.out.print(node + " "); 
        
        // Explore all the neighbors of the current node
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                // Recursively call the helper function for unvisited neighbors
                dfsHelper(graph, neighbor, visited); 
            }
        }
    }
}
```

## Adjacency Matrix

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

It is represented in the following way:

```java
int[][] graph;
```

The nodes connections are:

* graph\[i]\[j] = 1, the nodes are connected
  * if i==j, it means the node is connected to itself
* graph\[i]\[j] = 0, there is no connection between nodes

### Time and Space Complexity

| Complexity       | Best Case | Worst Case |
| ---------------- | --------- | ---------- |
| Time Complexity  | O(V^2)    | O(V^2)     |
| Space Complexity | O(V)      | O(V)       |

The best and worst time complexity for DFS using an adjacency matrix is the same, which is `O(V^2)`, where V is the number of vertices in the graph. This is because we need to visit every vertex and check its adjacency matrix row, which takes `O(V)` time for each vertex.

The space complexity is `O(V)` as we need additional space to store the visited array, which has a size equal to the number of vertices. The space complexity is independent of the density of the graph because we always allocate space for all vertices.

It's important to note that the space complexity may vary if additional data structures or recursive calls are used within the DFS algorithm. The analysis provided here considers the space complexity of the core DFS algorithm with an adjacency matrix.

### Code

```java
class Solution {
    public void dfs(int[][] graph) {
        if(graph ==null) {
            return 0;
        }
        int n = graph.length, count = 0;
        boolean[] visited = new boolean[n];

        for(int i=0;i<n;i++) {
            if(!visited[i]) {
                dfsHelper(i, visited, graph);
            }
        }
        return;
    }

    private void dfsHelper(int index, boolean[] visited, int[][] graph) {
        visited[index] = true;

        for(int i=0;i<graph.length;i++) {
            if(graph[index][i]==1 && !visited[index]) {
                dfsHelper(index, visited, graph);
            }
        }
    }
}
```
