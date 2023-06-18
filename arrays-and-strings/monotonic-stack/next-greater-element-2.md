# Next Greater Element 2

### Problem:

{% embed url="https://leetcode.com/problems/next-greater-element-ii/description/" %}

### Solution:

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        // Time Complexity: O(n+n) = O(2n) = O(n), 2n cause we iterate through twice
        // Space Complexity: O(n), Storage space for stack
        // Edge case
        if(nums == null) {
            return null;
        }

        /*
        first loop is to find the next greater element for each element of the array in its
        right side (i.e., for the elements present on the right side of the current element)

        second loop is to find the next greater element for each element of the array in its
        left side (i.e., for the elements present on the left side of the current element).
        */

        int[] result = new int[nums.length];
        Arrays.fill(result, -1); // Initialise result set with -1
        Stack<Integer> stack = new Stack<>();

        // Loop once, we can get the Next Greater Number of a normal array.
        // Loop twice, we can get the Next Greater Number of a circular array
        for(int j=0;j<2;j++) {                
            for(int i=0;i<nums.length;i++) {
                // Maintaining strictly increasing monotonic stack
                while(!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
                    int stackTop = stack.pop();
                    result[stackTop] = nums[i];
                }
                stack.push(i);
            }
        }
        return result;
    }
}
```
