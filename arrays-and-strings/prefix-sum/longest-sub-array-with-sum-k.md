# Longest Sub-Array with Sum K

### Problem

[https://practice.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1](https://practice.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1)

<figure><img src="../../.gitbook/assets/image (28) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (11) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Solution

```java
// Function for finding maximum and value pair
public static int lenOfLongSubarr (int nums[], int N, int K) {
    // N -> Length of nums
    // Time complexity: O(n)
    // Space complexity: O(n), n -> Space taken by HashMap
    int prefix = 0, maxLen = 0;
    
    // Store the prefix sum with the position
    Map<Integer, Integer> map = new HashMap<>();
    
    for(int i=0;i<N;i++) {
        prefix += nums[i];
        
        // To handle the case where the prefix sum itself equals to target sum
        if(prefix == K) { 
            // Then the longest subarray is the current position itself
            maxLen = i+1;
        }
        
        // Checking the complement and last position where it occurred
        // to calculate the maximum length of the subarray
        int complement = prefix - K;
        if(map.containsKey(complement)) {
            maxLen = Math.max(maxLen, i - map.get(complement));
        }
        
        // Putting prefix as we check if the prefix is found in previous positions
        map.putIfAbsent(prefix, i);
    }
    return maxLen;
}
```

