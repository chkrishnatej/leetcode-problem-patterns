# Ceiling - 744. Find Smallest Letter Greater Than Target

## Problem

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

{% content-ref url="../../concepts/floor-and-ceiling-of-number.md" %}
[floor-and-ceiling-of-number.md](../../concepts/floor-and-ceiling-of-number.md)
{% endcontent-ref %}

```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        // n -> Length of letters array
        // Time complexity: O(log n)
        // Space complexity: O(1), auxiliary space
        int start = 0, end = letters.length - 1;

        // Even though the given input contains characters, 
        // comparative operations can be done.
        while(start <= end) {
            int mid = start + (end-start)/2;

            // In the question, the asked for smallest letter greater than 
            // given target. So we dont handle the equal case seperately.

            // If the mid letter is less than/equal to target, according to 
            // the question, we still need to find the greater one than target
            // so we look forward, i.e why the condition is modified
            // as letters[mid] <= target
            if(letters[mid] <= target) {
                start = mid + 1;
            } 
            // If the target is less than mid element, then we might not be 
            // landing in smallest greater number, so jusst reduce the 
            // search space
            else if(target < letters[mid]) {
                end = mid - 1;
            } 
        }

        // The question tells if there is no valid smallest letter greater
        // than target, then return first character.
        // This occurs when start == letters.length, so to return "0",
        // when the start == letters.length, then it returns "0", cause reminder
        // is zero
        return letters[start % letters.length];
    }
}
```
