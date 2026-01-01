# Boyer-Moore Voting Algorithm

## Questions

1. [Majority Element - Leetcode](https://leetcode.com/problems/majority-element/)
2. [Majority Element II - Leetcode](https://leetcode.com/problems/majority-element-ii/)

## Variations

1. **Find majority element (> n/2) :** the classic.
2. **Find elements > n/3** : extended version for more than one majority.
3. **Majority in a stream :** constant space, streaming data.
4. **Weighted votes** : when counts aren’t equal.
5. **Verification step :** avoid false positives when no majority exists.

***

## Naive Algorithm

{% hint style="success" %}
In a given array, if there exists a majority element find it i.e. $$n/2$$
{% endhint %}

The general idea of solving the problem is:

1. Create a HashMap which stores frequencies of each element
2. Iterate through the input array
3. Update the frequency of each element
4. Find the threshold i.e. $$n/2$$
5. Iterate over the HashMap and keep checking if any item's frequency is **greater** than the threshold
6. Return the item value whose frequency is **greater than threshold**&#x20;

### Code

```java
import java.util.HashMap;
import java.util.Map;

public class MajorityElement {
    public static int findMajorityElement(int[] nums) {
        // 1. Create a HashMap to store frequencies
        HashMap<Integer, Integer> map = new HashMap<>();
        int n = nums.length;

        // 2 & 3. Iterate through array and update frequencies
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        // 4. Find the threshold (n/2)
        int threshold = n / 2;

        // 5 & 6. Iterate over the HashMap and check frequency
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() > threshold) {
                return entry.getKey();
            }
        }

        // Return -1 or throw exception if no majority element exists
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {2, 2, 1, 1, 1, 2, 2};
        int result = findMajorityElement(arr);

        if (result != -1) {
            System.out.println("The majority element is: " + result);
        } else {
            System.out.println("No majority element found.");
        }
    }
}
```

### Time and Space Complexity

{% hint style="info" %}
$$n$$ ➔ Length of the array\
$$k$$ ➔ Size of the Hashmap \
**Point to note:** $$n\ge k$$&#x20;
{% endhint %}

* **Time Complexity:** $$O(n)$$
  * Iterating over the array once - $$O(n)$$
  * Iterating over the hashmap - $$O(k)$$
  * Final = $$O(n) + O(k) = O(n)$$
* **Space Complexity:** $$O(k)$$

### Interview Setting Limitations

* _"Your solution is_ $$O(n)$$ _time and_ $$O(n)$$ _space. Can you do it in_ $$O(1)$$ _space?"_
* _"What if the array is so large it doesn't fit in memory, but we can stream it? How would you find the majority element without storing every unique value?"_
* _"Assume the majority element **always exists** in the input."_
* _"Imagine you are processing a stream of data and can only keep one or two variables in memory at a time."_

***

## Boyers-Moore Voting Algorithm

These limitations can be solved using "_**Boyers-Moore Voting Algorithm**_"

### Crux of the algorithm

> Since the majority element appears more than half the time, it can 'outvote' all other elements combined. If we pair up two different elements and remove them, the majority element will still be the one remaining at the end.

{% hint style="info" %}
The above algorithm is valid for majority element i.e. $$n/2$$
{% endhint %}

### The Core Intuition: Pairing Off

The algorithm works by trying to pair up different elements and removing them from the total count.

* If you have two different numbers, they cancel each other out.
* Because the majority element appears more than 50% of the time, even if every other number in the array "sacrifices" itself to cancel out one instance of the majority element, there will still be at least one majority element left standing at the end.

### The Logic of the "Counter"

The counter isn't actually counting the frequency of a number; it is counting the "surplus" of the current candidate.

* When `count == 0`: It means all previous elements have been paired off and cancelled out. We pick the next available number as our new `candidate`.
* When we see the same number: We increment the count (the surplus increases).
* When we see a different number: We decrement the count (one-to-one cancellation).

***

Basically&#x20;

* A HashMap tracks _everything_ (full state).
* Boyer-Moore tracks only the _relationship_ between elements (minimal state).
* You are essentially saying: "I don't need to know how many times _every_ number appeared; I only need to know which number is currently 'winning' the cancellation war."

> **Crucial Note:** This algorithm only guarantees finding the majority element if one actually exists. If there's a chance the array has no majority, you must perform one final pass through the array at the end to verify your candidate's total count is $$> n/2$$.

### Code

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

### Time and Space Complexity

{% hint style="info" %}
$$n$$ ➔ Length of the array
{% endhint %}

* **Time Complexity:** $$O(n)$$
  * Iterates twice over the input array $$O(n) + O(n) = O(2n) = O(n)$$
* **Space Complexity:** $$O(1)$$
  * No extra space is used

***

### Twist the scenarios

1. **Streaming (single pass, no verification)**
   * Use **Misra-Gries** (generalized BMV): maintain $$k-1$$ candidates with counts.
   * **Guarantees:** any element with $$freq\gt (n/k)$$ appears in candidates.
   * _**Trade-off**_**:** false positives possible; no strict majority guarantee without 2nd pass.
2. **Big Data/Parallel Streaming**
   1. **Local Sketching**: Each machine independently computes a _Misra-Gries summary_ (size _k−1_) over its partition — capturing potential heavy hitters with bounded error and no false negatives.
   2. **Global Aggregation**: All local summaries are merged (via shuffle/group-by) into a single multiset of candidate–count pairs — total size is _O(k × machines)_.
   3. **Global Sketching**: Run Misra-Gries _again_ on the merged candidate set to produce the final ≤ _k−1_ candidates — guaranteed to include any element with frequency > _n/k_.

***

## Boyers-Moore Algorithm for $$n/3$$

It can be achieved by tracking two items and two counts

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

### Time and Space Complexity

{% hint style="info" %}
$$n$$ ➔ Length of the array
{% endhint %}

* **Time Complexity:** $$O(n)$$
  * Iterates twice over the input array $$O(n) + O(n) = O(2n) = O(n)$$
* **Space Complexity:** $$O(1)$$
  * No extra space is used

***

## **Misra-Gries Heavy Hitters vs Count-Min Sketch for** $$n/k$$ items

It is generally not expected to implement Misra-Gries algorithm

We can say:

> For arbitrary k, I’d switch to a parameterized Misra-Gries sketch — maintain a map of up to k−1 candidates, decrement all on overflow. It’s composable, so we can merge per-machine sketches in a reduce phase. BMV is just k=2

* **Misra-Gries:** Better for identifying exact items (if a second pass is possible).
* **Count-Min Sketch:** Better for frequency estimation and handling deletes (using a "Count-Min Sketch with conservative update" or "Longest Frequent Elements").

#### Technical Refinement:

Ensure you explicitly state that in the "reduce" phase, merging two sketches involves summing counts of common elements and then decrementing counts by the minimum shared values to maintain the $$1/k$$ property.
