# Topological Sort

Topological sort helps in scheduling job, resolving dependencies order and identifying dead locks. It can be done using both **DFS** and **BFS**

&#x20;For topological sort to be done, the graph should be **Directed Acyclic Graph (DAG)**, i.e. no cycles should be found in the graph.

{% hint style="info" %}
Topological sort can help us identify if the graph is DAG or not. If the end result size is not equal to total number of courses, then there exists a cycle in given graph.
{% endhint %}



![](<../../.gitbook/assets/image (66).png>)

## DFS



```java
public class TopologicalSortDFS {
  /**
   * Performs a topological sort of a directed graph.
   *
   * @param adj The adjacency matrix of the graph.
   * @return A list of the vertices in topological order.
   */

  public static List<Integer> topoSort(int[][] adj) {

    // Create a visited array to track which vertices have been visited.
    boolean[] visited = new boolean[adj.length];

    // Create a stack to store the topological order of the vertices.
    Stack<Integer> stack = new Stack<>();

    // Iterate over each vertex in the graph.
    for (int i = 0; i < adj.length; i++) {

      // If the current vertex has not been visited, then recursively call the topo function.
      if (!visited[i]) {
        topo(adj, i, visited, stack);
      }
    }

    // Create a list to store the vertices in topological order.
    List<Integer> result = new ArrayList<>();

    // Iterate over the stack and add the vertices to the list.
    while (!stack.isEmpty()) {
      result.add(stack.pop());
    }

    // Return the list of vertices in topological order.
    return result;
  }

  /**
   * Recursively performs a topological sort of a directed graph.
   *
   * @param adj The adjacency matrix of the graph.
   * @param u The current vertex.
   * @param visited The visited array.
   * @param s The stack to store the topological order of the vertices.
   */

  private static void topo(int[][] adj, int u, boolean[] visited, Stack<Integer> stack) {

    // Mark the current vertex as visited.
    visited[u] = true;

    // Iterate over each adjacent vertex.
    for (int v : adj[u]) {

      // If any adjacent vertex has not been visited, then recursively call the topo function.
      if (!visited[v]) {
        topo(adj, v, visited, stack);
      }
    }

    // Push the processed node to stack
    stack.push(u);
  }
}
```

## BFS (Khan's Algorithm)

Kahn's algorithm is a topological sorting algorithm that works by repeatedly adding vertices with an indegree of 0 to a queue. Once all of the vertices have been added to the queue, the algorithm removes them from the queue in the order that they were added. This gives us a topological order of the graph.

### Indegree

Indegree is the number of edges that are directed into a vertex. A vertex with an indegree of 0 is a vertex that has no incoming edges.

> The significance of the indegree is to determine the number of prerequisites or dependencies for each course or node in a directed graph.

#### **Significance of indegree**

* By identifying **the nodes with an indegree of 0**, we can find the nodes that have no prerequisites or dependencies. These nodes can be processed first since they do not rely on any other nodes.&#x20;
* By enqueuing the nodes with an indegree of 0, the algorithm begins by handling the nodes that have no dependencies, gradually progressing towards the nodes with higher indegrees.

#### Significance of decrementing the in degree

* By decrementing the indegree of a node, we indicate that one of its dependencies has been fulfilled or processed.
* Decrementing the indegree helps keep track of the remaining prerequisites for each node.
* When the indegree of a node becomes 0, it means that all its prerequisites have been satisfied, and the node becomes eligible for processing.
* Nodes with an indegree of 0 are enqueued and processed first since they have no dependencies or prerequisites.
* The process continues, gradually processing nodes with higher indegrees as their prerequisites are fulfilled.
* Decrementing the indegree ensures that nodes are processed in the correct order based on their prerequisites, leading to a valid topological ordering.

### Code

```java
public class KahnsAlgorithm {

   public static List<Integer> topologicalSort(int n, int[][] graph) {
      // Initialize the indegree array.
      int[] indegree = new int[n];

      Map < Integer, List<Integer>> graph = new HashMap<>();

      for (int i = 0; i < n; i++) {
         graph.put(i, new ArrayList<>());
         ancestorList.add(new TreeSet<Integer>());
      }

      for (int[] edge: edges) {
         int source = edge[0];
         int dest = edge[1];
         graph.get(source).add(dest);
         indegree[dest]++;
      }

      // Create a queue to store the vertices with zero indegree.
      Queue<Integer> queue = new LinkedList<>();

      // Iterate over all vertices in the graph.
      for (int i = 0; i < indegree.length; i++) {
         // If the indegree of the vertex is zero, add it to the queue.
         if (indegree[i] == 0) {
            queue.add(i);
         }
      }

      // While the queue is not empty, do the following:
      List<Integer> topologicalOrder = new ArrayList<>();

      while (!queue.isEmpty()) {
         // Pop the first vertex from the queue.
         int v = queue.poll();

         // Iterate over all edges in the graph that start at the current vertex.
         for (int u: graph[v]) {
            // Decrement the indegree of the destination vertex.
            indegree[u]--;

            // If the indegree of the destination vertex is now zero, add it to the queue.
            if (indegree[u] == 0) {
               queue.add(u);
            }
         }

         // Add the vertex to the topological order.
         topologicalOrder.add(v);
      }

      // Return the topological order.
      return topologicalOrder;
   }
}
```
