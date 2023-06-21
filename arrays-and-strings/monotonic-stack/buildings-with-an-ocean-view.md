# 💰 Buildings With an Ocean View

### Problem

{% embed url="https://leetcode.com/problems/buildings-with-an-ocean-view/description/" %}

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

### Solution

```java
class Solution {
    public int[] findBuildings(int[] heights) {
        // Time Complexity: O(n), iterate through heights
        // Space Complexity: O(n), storage for stack
        Stack<Integer> stack = new Stack<>();

        // Here we are intersted in the strictly decreasing heights of building and
        // no other metric, so we can return the builidngs height indexes directly
        for(int i=0;i<heights.length;i++) {
            // As the building can have an ocean view iff there are no obsructions
            // So to make sure of that, when we are traversing from left->right,
            // the building heights have to be strictly decreasing
            while(!stack.isEmpty() && heights[stack.peek()] <= heights[i]) {
                stack.pop();
            }
            stack.push(i);
        }
        return stack.stream().mapToInt(i->i).toArray();
        
    }
}
```
