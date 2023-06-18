# Next Greater Element 1

### Problem:

{% embed url="https://leetcode.com/problems/next-greater-element-i/description/" %}

### Solution:

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        // Time complexity: O(nums1.length + nums2.length)
        // Space complexity: O(nums2.length)

        // Storing it in map to retrieve the next greater element of current element
        // And the numbers are unique
        Map<Integer, Integer> map = new HashMap<>();
        Stack<Integer> stack = new Stack<>();

        for(int num:nums2) {
            // Maintaining strictly increasing monotonic stack
            while(!stack.isEmpty() && stack.peek() < num) {
                map.put(stack.pop(), num); // Instead of storing indexes, directly storing values
            }
            stack.push(num);
        }

        int[] result = new int[nums1.length];
        for(int i=0;i<nums1.length;i++) { // Traverse through nums1
            // As the nums1 is subset of nums2, we can call it from map
            // If any number is not present, the value is -1
            result[i] = map.getOrDefault(nums1[i], -1);
        }
        return result;
    }
}
```
