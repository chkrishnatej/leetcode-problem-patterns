# Weighted Majority Vote

## Time and Space Complexity <a href="#time-and-space-complexity-1" id="time-and-space-complexity-1"></a>

{% hint style="info" %}
$$n$$ ➔ Length of the array
{% endhint %}

* **Time Complexity:** $$O(n)$$
  * Iterates twice over the input array $$O(n)+O(n)=O(2n)=O(n)$$
* **Space Complexity:** $$O(1)$$
  * No extra space is used

## Key Modifications

* **Counter Logic:** The standard increment/decrement of `1` is replaced by `weights[i]`.
* **Accumulation:** `weightCounter` uses `long` to prevent overflow when summing large weights.
* **Threshold:** Verification is against `totalWeight / 2` rather than `n / 2`.

## Code

```java
class Solution {
    /**
     * Finds the majority element where each element has an associated weight.
     * Requirement: Weight(element) > (TotalWeight / 2)
     */
    public int weightedMajority(int[] nums, int[] weights) {
        if (nums == null || weights == null || nums.length != weights.length || nums.length == 0) {
            throw new IllegalArgumentException("Invalid input: Arrays must be non-empty and of equal length.");
        }

        long weightCounter = 0;
        int candidate = 0;

        // Phase 1: Weighted Candidate Identification
        for (int i = 0; i < nums.length; i++) {
            if (weightCounter == 0) {
                candidate = nums[i];
                weightCounter = weights[i];
            } else if (nums[i] == candidate) {
                weightCounter += weights[i];
            } else {
                weightCounter -= weights[i];
            }
        }

        // Phase 2: Verification
        long totalWeight = 0;
        long candidateWeight = 0;

        for (int i = 0; i < nums.length; i++) {
            totalWeight += weights[i];
            if (nums[i] == candidate) {
                candidateWeight += weights[i];
            }
        }

        if (candidateWeight > totalWeight / 2) {
            return candidate;
        }

        throw new RuntimeException("No weighted majority element found.");
    }
}
```
