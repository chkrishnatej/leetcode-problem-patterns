# Daily Temperatures

### Problem:

{% embed url="https://leetcode.com/problems/daily-temperatures/description/" %}

### Solution:

```java
class Solution {
    public int[] dailyTemperatures(int[] temps) {
        // Time Complexity: O(n), iterating through loop once
        // Space Complexity: O(n), storage for stack
        int[] result = new int[temps.length];
        Arrays.fill(result, 0);
        Stack<Integer> stack = new Stack<>();

        for(int i=0;i<temps.length;i++) {
            // Maintain strictly increasing monotonic stack
            while(!stack.isEmpty() && temps[stack.peek()] < temps[i]) {
                int stackTop = stack.pop();
                result[stackTop] = i - stackTop;
            }
            stack.push(i);
        }
        return result;
    }
}
```
