# CN: Implement upper bound

## Problem

{% embed url="https://www.naukri.com/code360/problems/implement-upper-bound_8165383" %}

## Intuition

It is the same as [cn-implement-lower-bound.md](cn-implement-lower-bound.md "mention"). But with a different condition. If we find a `num[mid] > target` then we will set the result as mid and move the high towards left, i.e.`high = mid-1`/



```java
// When we find the mid number is greater than target, then result is set to mid
// And check towards left to find the smallest greater number than target
  if(nums[mid] > x) { 
    result = mid;
    high = mid-1;
 } else {
     low = mid + 1;
 }
```

## Time Complexity

`O(log n)`

## Space Complexity

`O(1)`

## Solution

```java
public class Solution {

  /**
   * This method finds the upper bound of a target value in a sorted array.
   *
   * The upper bound is the index of the first element that is strictly greater than the target value.
   * If no element is strictly greater, the upper bound is equal to the length of the array.
   *
   * @param nums The sorted integer array to search in.
   * @param x The target value to search for.
   * @param n The length of the array.
   * @return The index of the upper bound of the target, or n if not found.
   */
  public static int upperBound(int[] nums, int x, int n) {

    // Initialize low and high pointers for binary search
    int low = 0;
    int high = n - 1;

    // Loop until the low pointer crosses the high pointer (search space exhausted)
    while (low <= high) {

      // Calculate the middle index to check
      int mid = low + (high - low) / 2;

      // Check if the target is found at the middle index
      if (nums[mid] > x) {
        // Update potential upper bound and search left half for stricter bound
        result = mid;
        high = mid - 1;
      } else {
        // Search right half for a strictly greater element
        low = mid + 1;
      }
    }

    // Return the upper bound (either the found index or array length if not found)
    return result;
  }
}
```
