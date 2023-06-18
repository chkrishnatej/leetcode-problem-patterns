# All Ancestors of a Node in a Directed Acyclic Graph

## Problem

{% embed url="https://leetcode.com/problems/all-ancestors-of-a-node-in-a-directed-acyclic-graph/description/" %}

## Intuition

It can be modelled as topological sort problem. Basically we need to find the ancestors of each node. And when traversing from one node to another

> 1 -> 3 -> 5

All the ancestors of 3 and 1 are also ancestors of 5. It is a basic traversal using Kahns algorithm, where we keep track of processed node



## Code

```java
class Solution {
    public List<List<Integer>> getAncestors(int n, int[][] edges) {
        // N -> Number of
        // T.C : 
        // S.C : O(n)
        List<List<Integer>> result = new ArrayList<>(); // Result container
        List<TreeSet<Integer>> ancestors = new ArrayList<>(); // Ancestors for a given node

        // Modelling the edges to adjacent map
        Map<Integer, List<Integer>> graph = new HashMap<>();

        // Init graph and ancestors
        for(int i=0;i<n;i++) { 
            graph.put(i, new ArrayList());
            ancestors.add(i, new TreeSet());
        }

        int[] indegree = new int[n]; // Keeps track of number of incoming nodes to current node

        // Populating graph and indegree
        for(int[] edge: edges) {
            int source = edge[0];
            int dest = edge[1];
            graph.get(source).add(dest);
            indegree[dest]++;
        }

        Deque<Integer> queue = new ArrayDeque<>();

        // Add all the node which does not have ancestors to the queue for processing 
        for(int i=0;i<n;i++) {
            if(indegree[i]==0){
                queue.offer(i);
            }
        }

        // Dont stop till you process all the node in the queue
        while(!queue.isEmpty()) {
            int parentNode = queue.poll();

            // Explore the parent node childs
            for(int childNode: graph.get(parentNode)) {
                // Check if the parent node has any ancestors
                if(!ancestors.get(parentNode).isEmpty()) {
                    // If so, add those ancestors to childNode
                    // Cz, as per def, ancestor means it can reach from any set of edges
                    ancestors.get(childNode).addAll(ancestors.get(parentNode));
                }
                // Add the parent node as ancestor to the child node
                ancestors.get(childNode).add(parentNode);
                // As we processed one prerequsite, decrement the indegree of childnode
                indegree[childNode]--;

                // If all the childnode prerequsites are done, 
                // add it to queue for processing
                if(indegree[childNode] == 0) {
                    queue.offer(childNode);
                }
            }
        }

        // Convert the treeset to array list and add to the result
        for(var ancestorPath: ancestors) {
            result.add(new ArrayList(ancestorPath));
        }

        return result;
    }
}
```
