# Possible Bipartition

## Problem

{% embed url="https://leetcode.com/problems/possible-bipartition/description/" %}

## Intuition

The ideas we perform in normal DFS operation on the graph but with a little twist. When we are performing the DFS operation, we also send an extra parameter called label.

In this problem will have a three states or three labels zero being unlabelled, 1 is label1 and -1 is label2.

Send label 1 to the DFS and we toggle it using a negative symbol inside the DFS function so that the label changes at every iteration. When we encounter any labelled node which has the same label as we are trying to label now then we can safely say that the graph cannot be bipartitioned



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

To store the labels of nodes in an array
{% endhint %}

## Code

```java
class Solution {
    public boolean possibleBipartition(int n, int[][] dislikes) {
        // V -> Vertices, E -> Edges
        // T.C : O(V+E)
        // S.C : O(V)

        // Understanding the input, it is a nested array
        // the first array tells about the person 
        // and second array tells about neighbors who should not be connected 
        
        // We have 3 state of labels
        // O -> Unlabelled
        // 1 --> label 1
        // -1 --> label 2

        // The basic idea is no neigbouring nodes should have same label after processing
        // or labelling all the nodes
        Map<Integer, List<Integer>> graph = new HashMap<>();

        // Populating the graph
        for(int i=1;i<=n;i++) {
            graph.put(i, new ArrayList());
        }

        for(int[] dislike: dislikes) {
            graph.get(dislike[0]).add(dislike[1]);
            graph.get(dislike[1]).add(dislike[0]);
        }

        // Initialises array with all zeros, which means unlabelled nodes
        int[] labelled = new int[n+1];

        // Iterating through all nodes, since there could be disjoint graph, 
        // using for loop to  ensure all nodes of all nodes are processed
        for(int i=1;i<=n;i++) {
            // While processing check the following:
            //    * Node is not processed yet, i.e 
            //    * Is the adjacent node with same label
            if(labelled[i]==0 && !isBipartite(i, labelled, graph, 1)) {
                return false;
            }
        }
        return true;
    }

    private boolean isBipartite(int person, int[] labelled, Map<Integer, List<Integer>> graph, int label) {
        // If the person is already labelled
        if(labelled[person]!=0) {
            // Since, we have an undirected graph of DISLIKES, what it means is the current person
            // is coming from a parent who are not supposed to be in the same group.
            // disliked graph are adjacent nodes
            return labelled[person] == label;
        }

        labelled[person] = label; // Marking the unclabelled node to a label

        for(int next:graph.get(person)){ // Get the neighbours
            // Process them by labelling them using a "-" toggle
            if(!isBipartite(next, labelled, graph, -label)) {
                return false;
            } 
        }
        // If its processed successfully, then it is a bipartite graph
        return true;
    }
}
```
