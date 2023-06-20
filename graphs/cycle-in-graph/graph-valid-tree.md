# Graph Valid Tree

### Problem

{% embed url="https://leetcode.com/problems/graph-valid-tree/description/" %}

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

> V -> vertices of the graph (nodes)
>
> E -> Edges (connection path from vertices)

## Time Complexity

{% hint style="info" %}
**O(|V| + |E|)**

This is because the algorithm needs to visit every vertex in the graph and explore all of its edges.

The algorithm visits every vertex in the graph by recursively calling itself on each vertex. The algorithm explores all of the edges in the graph by following the edges from each vertex to its neighbors.
{% endhint %}

## Space Complexity

{% hint style="success" %}
**O(|V|)**

To store a visited array of size |V|.

But it we consider the space taken by adjacency list, then it will be&#x20;

**O(|V| + |E|)**
{% endhint %}

### Code

### Graph Valid tree

```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        // Time Complexity: O(|V| + |E|)
        // V -> Number of vertices
        // E -> Edges of edges
        // It has to visit all the vertices(nodes) and all the edges(connections)
        // Space Complexity: O(V), stores all the vertices in the adjacency list
        
        if(n==0) {
            return true;
        }

        // Building adjacency list
        Map<Integer, List<Integer>> map = new HashMap<>();

        // Populating adjacency list
        for(int i=0;i<n;i++) {
            map.put(i, new ArrayList<>());
        }

        for(int[] edge:edges) {
            map.get(edge[0]).add(edge[1]);
            map.get(edge[1]).add(edge[0]);
        }
        
        // Keeping track of visited nodes
        boolean[] visited = new boolean[n];

        // Check if any cycles or disconnected components are present in graph
        if(!dfs(0, visited, map, -1)) {
            // If it has cycle or disconnected component, it cannot be a tree
            return false;
        }

      
        for(boolean b: visited) {
            // If any of the nodes are not processed after dfs, 
            // then there is another disconnected component, 
            // so it cannot be a tree
            if(!b) {
                return false;
            }
        }

        //If none of the above conditions apply, then the graph is a valid tree
        return true;
    }

    private boolean dfs(int index, boolean[] visited, Map<Integer, List<Integer>> graph, int parent) {
        visited[index] = true;

        for(int neighbour: graph.get(index)) {
            // If the parent is encountered while traaversing, 
            // do nothing, just check other neighbours
            if(neighbour == parent) {
                continue;
            }

            // If neighbour which has already been processed, comes up again
            // for processing, then it is cyclic
            if(visited[neighbour]) {
                return false;
            }

            // If any of the nodes are disconnected from the current graph, 
            // a tree cannoot have disconnected  nodes
            if(!dfs(neighbour, visited, graph, index)) {
                return false;
            }
        }
        // If the given graph is successfully processed, then it is a valid graph 
        // tree
        return true;
    }
}
```
