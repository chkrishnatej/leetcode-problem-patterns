# Classic Boyers-Moore for n/2

[Majority Element - Leetcode](https://leetcode.com/problems/majority-element/)

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
public class MajorityElementSolver {
    /**
     * Finds the element that appears more than n/2 times.
     * Returns -1 if no such element exists.
     */
    public int findMajority(int[] nums) {
        if (nums == null || nums.length == 0) {
            return -1;
        }

        int candidate = 0;
        int count = 0;

        // Phase 1: Candidate Selection
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
                count = 1;
            } else if (num == candidate) {
                count = count + 1;
            } else {
                count = count - 1;
            }
        }

        // Phase 2: Mandatory Verification
        // Boyer-Moore only guarantees a candidate; it doesn't guarantee majority.
        int actualCount = 0;
        for (int num : nums) {
            if (num == candidate) {
                actualCount = actualCount + 1;
            }
        }

        if (actualCount > nums.length / 2) {
            return candidate;
        }

        return -1;
    }
}
```
