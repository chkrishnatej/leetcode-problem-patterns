---
description: A graph of two disjoint sets
---

# Bipartite Graph

### What is a bipartite graph?

* A **bipartite graph** is a graph whose vertices can be partitioned into two sets, such that no two adjacent vertices belong to the same set.
* A **bipartite graph** is a graph whose vertices can be partitioned into two sets, such that no two vertices in the same set are connected by an edge. In other words, a bipartite graph is a graph that does not contain any odd cycles.
* A **bipartite graph** is a graph which can be divided into two disjoint sets

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

The adjacent nodes (direct neighbours) cannot have same colour



## Properties of bipartite graph

1. **Vertex Partition:** A bipartite graph can be partitioned into two disjoint sets of vertices, say U and V, such that every edge in the graph connects a vertex from set U to a vertex from set V. This partition of vertices is called a bipartition.
2. **No Edges within Sets:** In a bipartite graph, there are no edges that connect vertices within the same set. In other words, all edges in the graph go from vertices in set U to vertices in set V, or vice versa.
3. **Coloring**: A bipartite graph can be colored using two colors, such that no two adjacent vertices have the same color. This is known as a proper vertex coloring.
4. **Independent Sets:** In a bipartite graph, the vertices in each set form an independent set, which means that there are no edges between vertices in the same set.
5. **No Odd Cycles:** A bipartite graph does not contain any odd-length cycles. All cycles in a bipartite graph have an even number of vertices.
6. **Maximum Bipartite Matching**: In a bipartite graph, the maximum matching (the largest possible set of mutually non-adjacent edges) can be found efficiently using algorithms such as the Hopcroft-Karp algorithm or the Ford-Fulkerson algorithm.

### Algorithm

This type of problems come up in graphs. Depth First Search is used to solve the problem.

* Create a list to keep track of colours
* Iterate it through total number of nodes
* In DFS, mark the initial node with colour `1`&#x20;
* Then visit its neighbouring nodes and toggle `1` using a negative sign.
* If we encounter a node, which has same colour as colour it is being assigned, then graph is not bipartite

### Code

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        
        // Initialises array with all zeros, which means uncoloured nodes
        int[] colouredNodes = new int[n]; // Keeps track of nodes which has been coloured
        
        /*
        Iterating through all nodes, since there could be disjoint graph, using for loop to           ensure all nodes of all graphs are processed
        */
        for(int i=0;i<n;i++) { 
            /*
            While processing check the following:
                * Node is not processed yet
                * Is the adjacent node with same colour
            */
            if(colouredNodes[i]==0 && !isValidColour(i, graph, colouredNodes, 1))
                return false;
        }
        return true;
    }
    
    private boolean isValidColour(int node, int[][] graph, int[] colouredNodes, int colour) {
        // Checks if adjacent node(which has already been processed) has same colour as parent
        if(colouredNodes[node]!=0) 
            return colouredNodes[node] == colour;
        
        colouredNodes[node] = colour; // Marking the uncoloured node to a colour
        
        for(int next:graph[node]) { // Get the neighbours
            // Process them by colouring them using a "-" toggle
            if(!isValidColour(next, graph, colouredNodes, -colour))
                return false;
        }
        // If its processed successfully, then it is a bipartite graph
        return true;
    }
}
```

### Problems

*
