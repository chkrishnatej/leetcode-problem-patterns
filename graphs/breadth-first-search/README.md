# Breadth First Search

Breadth First Search&#x20;









{% hint style="info" %}
### Time Complexity:

`O(V+E)`
{% endhint %}

### Template

```java
import java.util.*;

class Solution {
    public void bfs(int start, List<List<Integer>> adjList) {
        Queue<Integer> queue = new LinkedList<>();
        Set<Integer> visited = new HashSet<>();

        // Add the start node to the queue and mark it as visited
        queue.offer(start);
        visited.add(start);

        while (!queue.isEmpty()) {
            int size = queue.size();

            // Process nodes at the current level
            for (int i = 0; i < size; i++) {
                int curr = queue.poll();

                // Perform any necessary operations on the current node
                // ...

                // Add unvisited neighbors to the queue and mark them as visited
                for (int neighbor : adjList.get(curr)) {
                    if (!visited.contains(neighbor)) {
                        queue.offer(neighbor);
                        visited.add(neighbor);
                    }
                }
            }
        }
    }
}

```
