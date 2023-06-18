# Fixed Sliding Window

For fixed Sliding window problems, when we encounter a valid window, we need to check for the constraints, and if the constraints are satisfied, we can add the required data to result set.

### Template

```java
public void method(String s) {
        // Create a result set/variable
  
        // Check for edge cases
        if(s.length() == 0 || s == null || s.isBlank()) {
            // Handle it appropriately or return the result
        }
    
        // Create a ds, which is required to hold the data of the window
        holder = Map, Set, int[]

        int start = 0;
        // Iterate it over input
        for(int end=0;end<s.length();end++) {
            // Get the current element
            char ch = s.charAt(end);

            // Store the current element desired property in holder
            holder.put(ch, freq);

            // Check if the given window is valid
            // If valid, add the required property (like index, frequency, length) to result set

            // Reduce the size of window by moving forward start pointer
        }
        return result;
    }
```

### Example Problem:

[https://leetcode.com/problems/find-all-anagrams-in-a-string](https://leetcode.com/problems/find-all-anagrams-in-a-string)

You can see the above template being followed here

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        // Time Complexity: O(pLen + sLen) + O(26)
        // O(sLen) + O(1) => O(sLen)
        // Space complexity: O(K)
        // K - 26
        
        // Storing result
        List<Integer> result = new ArrayList<>(); 
        int pLen = p.length(), sLen = s.length();
        
        // Edge case
        if(sLen < pLen) 
            return result;
        
        // Freq Maps to store window values
        int[] sCount = new int[26]; 
        int[] pCount = new int[26];
        
        // Set the frequency of "p" string. This will unchanged
        for(char ch: p.toCharArray())
            pCount[ch-'a']++;
        
        // Traverse through "s"
        for(int end=0;end<sLen;end++) {
            // Get current character
            char current = s.charAt(end);
            
            // Update frequency of "current" in sMap
            sCount[current-'a']++;
            
            // When a window is found
            if(end >= pLen) {
                // Get the character at the start of the window and decrement it
                sCount[s.charAt(end-pLen) - 'a']--; 
            }
            // If the current window is valid, i.e. anagram 
            // The case is both frequencies should be the same, 
            // since order is not taken into consideration). 
            // If they are equal, then add the start of the current window index
            if(Arrays.equals(sCount, pCount)) {
                result.add(end-pLen+1);
            }
        }
        return result;
    }
}
```



