---
description: '#binary-tree'
---

# TODO: 987. Vertical Order Traversal of a Binary Tree

## Problem

[https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/)

## Intuition



## Time Complexity

<pre class="language-java"><code class="lang-java">O(n) - BFS
<strong>(O(vLevels) * O(log k)) +  (O(colLevels) * O(log k)) - Insertion of values into TreeMaps
</strong>O(vLevels) * (O(colLevels) * O(log k))- Processing the values in map for vertical and col level

k-> number of values in each colLevel
O(log k) -> Time complexity for sorting values

Total = O(n) + (O(vLevels) * (O(colLevels) * O(log k)))
</code></pre>

## Space Complexity

```java
O(vLevel) * O(colLevel) -> To store values in Map
O(n) -> Storing values in result
```

## Solution

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        // Time Complexity:
        // O(n) - BFS
        // O(vLevels) O(log k) * O(colLevels) * O(log k) - Insertion of values into TreeMaps
        // O(vLevels) * (O(colLevels) * O(log k))- Processing the values in map for vertical and col level
        // k-> number of values in each colLevel
        // O(log k) -> Time complexity for sorting values
        // Total = O(n) + (O(vLevels) * (O(colLevels) * O(log k)))

        // Space Complexity:
        // O(vLevel) * O(colLevel) -> To store values in Map
        // O(n) -> Storing values in result

        // Store the result
        List<List<Integer>> result = new ArrayList<>();

        // Handle edge case
        if(root==null) {
            return result;
        }

        // Map to store <VerticalLevel, <ColumnLevel, Values>>
        Map<Integer,Map<Integer,List<Integer>>> map = new TreeMap<>();
        // Queue to store the level order traversal
        Queue<Pair<Integer,TreeNode>> q = new ArrayDeque<>();

        q.offer(new Pair(0,root));// Add the root node

        int col = 0; // Keep track of level

        while(!q.isEmpty()) {
            int size = q.size();
            for(int i=0;i<size;++i) {
                Pair<Integer,TreeNode> pair = q.poll(); // Poll the node for processing
                int vLevel = pair.getKey(); // Vertical Level
                TreeNode node = pair.getValue(); // Node

                // Add nodes to queue for processing
                if(node.left!=null) {
                    q.offer(new Pair(vLevel-1,node.left));
                }
                if(node.right!=null) {
                    q.offer(new Pair(vLevel+1,node.right));
                }
                
                // Create a tmp map to store the column level info with values
                // The treemap ensures column in ascending manner is preserved
                Map<Integer,List<Integer>> tmp = new TreeMap<Integer,List<Integer>>();
                map.putIfAbsent(vLevel,tmp); // Add tmp if no vlevel is there
                map.get(vLevel).putIfAbsent(col,new ArrayList<>()); // Add new col with array list if not present
                map.get(vLevel).get(col).add(node.val); // Add the value to vertical -> column levels
            }
            ++col; // Increment the column (here col refers to x-axis)
        }

        // Process the map for building the result
        // The treemap ensures the vertical order in ascending manner is preserved
        for(Map<Integer,List<Integer>> tmp : map.values()) {
            List<Integer> lst = new ArrayList<>(); // Create a lst to store the vLevel values
            
            // Process the column level values
            for(List<Integer> tmp1 : tmp.values()) { 
                Collections.sort(tmp1); // Sort the values in same col level
                lst.addAll(tmp1);
            }
            result.add(lst); // Add to list
        }
        return result;
    }
}
```
