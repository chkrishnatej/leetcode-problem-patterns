# 132 pattern

### Problem

{% embed url="https://leetcode.com/problems/132-pattern/description/" %}

### Solution

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        int secondMax = Integer.MIN_VALUE;
        
        Stack<Integer> stack = new Stack<>();
        
        for(int i=nums.length-1; i>= 0; i--) {
            int current = nums[i];
            
            /* 
            If current number is lesser than secondMax which is found, then 132 
            pattern is found.
            Since the "32" pattern is found using "while" condition, if we have a 
            case of current < secondMax, as we are traversing from "right --> left", the 
            "i < j < k" is satisified
            
            And our secondMax is found on right side post "nums[j] which is max element",
            then "nums[i] < nums[k] < nums[j]" condition is also satisfied
                
            */
            if(current < secondMax)
                return true;
            
            /* If current is greater than top of stack, then the top most element in stack is
               second largest. Basically we are elimenating smaller values than largest
               number. And storing the removed elements as secondMax.
               
               We are trying to build "32" pattern here. The largest element exists
               on left side. 
            */
            while(!stack.isEmpty() && current > stack.peek())
                secondMax = stack.pop();
            
            stack.push(current);
        }
        return false;
    }
}
```
