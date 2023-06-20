# Find next greater indexes

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

```java
public class Main {
    public static void main(String[] args) {
        int[] nums = new int[]{13, 8, 1, 5, 2, 5, 9, 7, 6, 12};
        System.out.println(Arrays.toString(findNextGreaterIndexes(nums)));
    }
    
    private static int[] findNextGreaterIndexes(int[] nums) {
        // Next greater indexes mean the values should be strictly increasing, 
        // so we use ">"
        
        // Time Complexity: O(n), Iterating through all elements in array
        // Space Complexity: O(n), In worst case, we need to store all elements in stack
        
        // Edge case
        if(nums==null) {
            return null;
        }
        
        // Initialise an empty stack
        Stack<Integer> stack = new Stack<>();
        
        // Create a result int[] where we will store next greatest indexes
        int[] result = new int[nums.length];
        Arrays.fill(result, -1); // Initialise all the values in result with -1
        
        for(int i=0;i<nums.length;i++) { // Iterate through input array
            // nums[stack.peek()] --> Numerical value of index on top of stack
            // nums[i] --> current number
            // Loop through stack and check 
            // Since need to find strictly increasing index, we check 
            // if the numeric value of index stored in top of stack < than current number 
            // This would help us find strictly increasing index
            while(!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
                // As we found suitable value for the index on stack.peek(),
                // We pop it, which means we found greater index value for it
                int stackTopIdx = stack.pop();
                // Store the current index at stacktopidx positiaaon in result
                result[stackTopIdx] = i; 
            }
            // Push the current index to stack
            // By following above while loop, the monotonic property of stack is always maintained
            stack.push(i);
        }
        return result;
    }
}
```

### Understanding of \`while\` loop comparison

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

### Final Result

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>
