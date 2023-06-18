# Subarrays with K Different Integers

### Problem

[https://leetcode.com/problems/subarrays-with-k-different-integers/description/](https://leetcode.com/problems/subarrays-with-k-different-integers/description/)

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

### Solution

```java
class Solution {
    public int subarraysWithKDistinct(int[] nums, int k) {
        // n -> Length of nums
        // Time complexity: O(2n) --> O(n)
        // 2n because we would be traversing the array twice
        // exactly(K) = atMost(K) - atMost(K-1)
        // Space complexity: O(K) -- k -> number of distinct elements
        return helper(nums, k) - helper(nums, k-1);
    }

    private int helper(int[] nums, int k) {
        int count = 0;
        // Initialise map to keep count of frequencies integers in nums
        Map<Integer, Integer> freqMap = new HashMap<>();

        int start = 0; // Sliding window start

        // Iterate through nums with sliding window technique
        for(int end = 0;end<nums.length;end++) {
            // Add the current number frequency to map
            int current = nums[end];
            freqMap.put(current, freqMap.getOrDefault(current, 0)+1);

            // The condition is to have a subarray with the utmost "k" elements
            // If the condition is breached, then we need to get the 
            // frequency map to less or equal to "k" elements
            while(freqMap.size() > k) {
                // Get the element at windows start and reduce the frequency  by 1
                // And increment the window start pointer
                int windowStart = nums[start++];
                int updatedFrequency = freqMap.get(windowStart)-1;

                // If the frequency is zero, it cannot contribute to subarray
                // Remove it from the map
                if(updatedFrequency == 0) {
                    freqMap.remove(windowStart);
                } 
                else {
                    // Else update the frequency
                    freqMap.put(windowStart, updatedFrequency);
                }
            }
            // TODO: Need better explanation
            count += (end-start+1);
        }
        return count;
    }
    
    
}
```
