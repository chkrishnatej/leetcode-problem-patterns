# Top View

## Problem

{% embed url="https://www.naukri.com/code360/problems/top-view-of-binary-tree_799401" %}

## Intution

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

If we carefully observe the above picture, we can understand that, the top view of binary tree is the first node in at every horizontal distance.

Perform horizontal BFS traversal and update the value of map only when you encounter the horizontal distance for the first time.

This can be acheived using `map.putIfAbsent()`

## Time Complexity

`O(V+E)` -> Standard BFS time complexity

## Space Complexity

`O(K)`&#x20;

k-> maximum number of horizontal distances

## Code

```java
import java.util.*;

public class Solution {
    // Create class to store horizontal level and node
    static class Pair {
        int hLevel;
        TreeNode node;

        public Pair(int hLevel, TreeNode node) {
            this.hLevel = hLevel;
            this.node = node;
        }
    }


    public static List<Integer> getTopView(TreeNode root) {
        // Write your code here.
        List<Integer> result = new ArrayList<>();

        // In case, root is null, then return empty list
        if(root == null) {
            return result;
        }

        // Map<hLevel, firstElementInHLevel>>
        // Using treeMap so that order of horizontal distance is maintained
        // from [-minHLevel, +maxHLevel]
        Map<Integer, Integer> map = new TreeMap<>();
        
        // Create a queue to perform BFS
        Queue<Pair> q = new ArrayDeque<>();
        q.offer(new Pair(0, root));

        while(!q.isEmpty()) {
            int n = q.size();

            for(int i=0;i<n;i++) {
                // Standard BFS traversal
                Pair pair = q.poll();

                int hLevel = pair.hLevel;
                TreeNode node = pair.node;
                
                // While adding Pair, if left, -1 from current horizontal level
                if(node.left != null) q.offer(new Pair(hLevel-1, node.left));
                
                // While adding Pair, if right, +1 from current horizontal level
                if(node.right != null) q.offer(new Pair(hLevel+1, node.right));
                
                // Using putIfAbsent makes sure only the first value is picked
                map.putIfAbsent(hLevel, node.data);
            }
        }

        return new ArrayList(map.values());
    }
}
```

