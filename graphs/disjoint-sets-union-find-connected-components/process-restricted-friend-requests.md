# Process Restricted Friend Requests

## Problem

{% embed url="https://leetcode.com/problems/process-restricted-friend-requests/description/" %}

## Concept

This problem can easily be modelled to Union Find. We need to make connections and make sure that the current connection we are making does not fall in restricted connections.

It is a classic union find problem with an additional check on when we can make unions which is controlled by restrictions

## Algorithm

1. It creates a disjoint set data structure with `n` nodes.
2. It iterates over each request.
3. It checks if the two users involved in the request are already friends. If they are, then it does nothing.
4. It checks if the two users are connected by a restriction. If they are, then it cannot approve the friend request.
5. If the two users are not connected by a restriction, then it approves the friend request and merges the two connected components in the disjoint set data structure.
6. It returns the `result` array.

## Code

```java
class DisjointSet {
    List<Integer> parent = new ArrayList<>();
    List<Integer> rank = new ArrayList<>();

    public DisjointSet(int n) {
        for(int i=0;i<n;i++) {
            parent.add(i);
            rank.add(1);
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
            parent.set(parentX, parentY);
        } else if(rankY < rankX) {
            parent.set(parentY, parentX);
        } else {
            parent.set(parentY, parentX);
            rank.set(parentX, rankX + 1);
        }
    }
}
class Solution {
    public boolean[] friendRequests(int n, int[][] restrictions, int[][] requests) {
        // Time Complexity: O(m α(n)), inverse ackerman function
        // Space Complexity: O(n), n -> number of users in graph
        // `result` is a boolean array that will store the results of the friend requests.
        boolean[] result = new boolean[requests.length];

        // `ds` is a disjoint set data structure that will be used to keep track of the connected components of the graph.
        DisjointSet ds = new DisjointSet(n);

        for(int i=0;i<requests.length;i++) {
            // Get the two users representatives involved in the request.
            int p1 = ds.findParent(requests[i][0]);
            int p2 = ds.findParent(requests[i][1]);
            
            boolean flag = true;

            for(int[] restriction: restrictions) {
                // Get the restricted user representatives
                int r1 = ds.findParent(restriction[0]);
                int r2 = ds.findParent(restriction[1]);

                // If two users are connected by restriction, we cannot approve the request
                if((p1 == r1 && p2 == r2) || (p1 == r2 && p2 == r1)) {
                    flag = false;
                    break;
                }
            }
            // If the two users are not connected by a restriction, then we can approve the friend request.
            if(flag) {
                ds.unionByRank(p1, p2);
                result[i]  = true;
            }
        }
        return result;
    }
}
```
