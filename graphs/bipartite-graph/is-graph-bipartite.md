# Is Graph Bipartite?

## Problem:

{% embed url="https://leetcode.com/problems/is-graph-bipartite/description/" %}

## Intuition

The ideas we perform in normal DFS operation on the graph but with a little twist. When we are performing the DFS operation, we also send an extra parameter called colour.

In this problem will have a three states or three colours zero being uncoloured 1 is colour1 and -1 is colour2.

Send colour 1 to the DFS and we toggle it using a negative symbol inside the DFS function so that the colour changes at every iteration. When we encounter any coloured node which has the same colour as we are trying to colour now then we can safely say that the graph cannot be bipartitioned



## Time and Space Complexity

> V -> Vertices
>
> E -> Edges

{% hint style="info" %}
**Time Complexity: O(V+E)**

It is a standard DFS operation
{% endhint %}

{% hint style="success" %}
**Space Complexity: O(V)**

To store the color of nodes in an array
{% endhint %}

## Code

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        // V -> Vertices, E -> Edges
        // T.C : O(V+E)
        // S.C : O(V)
        int n = graph.length;

        // Understanding the input, it is a nested array
        // the first array tells about the node 
        // and second array tells about nodes connected to it
        // For ex, 
        //      graph == [[1,2,3],[0,2],[0,1,3],[0,2]]
        //      It means, node 1, i.e. the first int[] , it is connected to them

        
        // We have 3 state of colors
        // O -> Uncoloured
        // 1 --> color 1
        // -1 --> color 2

        // The basic idea is no two adjacent nodes should have same color after processing
        // or coloring all the nodes

        // Initialises array with all zeros, which means uncoloured nodes
        int[] colouredNodes = new int[n]; // Keeps track of nodes which has been coloured
        
        
        // Iterating through all nodes, since there could be disjoint graph, 
        // using for loop to  ensure all nodes of all nodes are processed
        for(int i=0;i<n;i++) { 
            // While processing check the following:
            //    * Node is not processed yet, i.e 
            //    * Is the adjacent node with same colour
            if(colouredNodes[i]==0 && !isValidColour(i, graph, colouredNodes, 1))
                return false;
        }
        return true;
    }
    
    private boolean isValidColour(int node, int[][] graph, int[] colouredNodes, int colour) {
        // If the node is colored
        if(colouredNodes[node] != 0) {
            // Check if the node color is same as the color we want to color the next node,
            // then the graph cannot be bipartite,
            return colouredNodes[node] == colour;
        }
        
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
