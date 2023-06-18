# Remove K digits

### Problem

{% embed url="https://leetcode.com/problems/remove-k-digits/description" %}

### Solution

```java
class Solution {
    public String removeKdigits(String nums, int k) {
        // Time Complexity: O(n), Iterating through nums.length
        // Space Complexity: O(n), Storage for stack and StringBuilder

        // Edge case
        if(nums.length()==k) {
            return "0";
        }
        
        // Monotonic stack
        Stack<Character> stack = new Stack<>();
        
        for(char num:nums.toCharArray()) {
            // When still the number of digits are there to be removed 
            // && stack is not empty
            // && the top element of stack is greater than current
            // Since we want to get smallest number possible, we remove
            // the biggest numbers from the stack. Thereby making the stack
            // strictly increasing 
            while(k > 0 && !stack.isEmpty() && stack.peek() > num) {
                stack.pop();
                k--;
            }
            stack.push(num);
        }

        // If after iterating the string, still the number of digits to be removed
        // are remaining, we remove the remaining elements
        while(k-- > 0) {
            stack.pop();
        }

        // Convert the remaining stack elements to form the number
        StringBuilder sb = new StringBuilder();

        for(char ch:stack) {
            if(ch == '0' && sb.length()==0) {
                continue;
            }
            sb.append(ch);
        }

        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```
