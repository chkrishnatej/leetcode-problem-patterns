# Classic Boyers-Moore for n/3

[Majority Element II - Leetcode](https://leetcode.com/problems/majority-element-ii/)

## Time and Space Complexity <a href="#time-and-space-complexity-1" id="time-and-space-complexity-1"></a>

{% hint style="info" %}
$$n$$ ➔ Length of the array
{% endhint %}

* **Time Complexity:** $$O(n)$$
  * Iterates twice over the input array $$O(n)+O(n)=O(2n)=O(n)$$
* **Space Complexity:** $$O(1)$$
  * No extra space is used

## Code

```java
class Solution {
    /**
     * Finds all elements that appear more than n/3 times.
     * Complexity: O(n) time, O(1) space.
     */
    public List<Integer> majorityElement(int[] nums) {
        // Validation of input constraints
        if (nums == null || nums.length == 0) {
            throw new IllegalArgumentException("Invalid input provided");
        }

        // Potential candidates and their respective confidence counters
        int count1 = 0, count2 = 0;
        // Assuming Integer.MIN_VALUE is not a part of the input
        int el1 = Integer.MIN_VALUE, el2 = Integer.MIN_VALUE;

        int n = nums.length;

        // Phase 1: Candidate Identification
        // Based on the principle that there can be at most two elements 
        // occurring more than n/3 times.
        for (int i = 0; i < n; i++) {
            if (count1 == 0 && el2 != nums[i]) {
                // Assign first candidate slot
                count1 = 1;
                el1 = nums[i];
            } 
            else if (count2 == 0 && el1 != nums[i]) {
                // Assign second candidate slot
                count2 = 1;
                el2 = nums[i];
            } 
            else if (el1 == nums[i]) {
                // Increment if current matches first candidate
                count1++;
            } 
            else if (el2 == nums[i]) {
                // Increment if current matches second candidate
                count2++;
            } 
            else {
                // Decrement both counters if current matches neither
                count1--;
                count2--;
            }
        }

        // Phase 2: Candidate Verification
        // The voting phase only identifies potential candidates; 
        // a second pass is required to confirm they meet the threshold.
        int threshold = (n / 3) + 1;
        List<Integer> result = new ArrayList<>();

        count1 = 0;
        count2 = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == el1) {
                count1++;
            } else if (nums[i] == el2) {
                count2++;
            }
        }

        // Add to result if frequency satisfies the n/3 requirement
        if (count1 >= threshold) {
            result.add(el1);
        }
        if (count2 >= threshold && el1 != el2) {
            result.add(el2);
        }

        return result;
    }
}
```
