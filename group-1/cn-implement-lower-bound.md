# CN: Implement Lower Bound

## Problem

{% embed url="https://www.naukri.com/code360/problems/binary-search_972" %}

## Intuition

We need to find a number that is present in the least index, if not find the least index where the number would fit.

It is not a traditional binary search problem. We are not searching for a particular target, but we are trying to find  where the given target would fit even if it exists are not.

The template is the same, but with little twist:

```java
int result = arr.length - 1; // Result is initialised at last index
// ...
// Checking if the given target is less than nums[mid] to find the lower bound
if(target <= nums[mid]) { 
    result = mid; // Then set the result to mid
    high = mid-1; // Then check towards left to see if any other number is lesser than the target
} else {
    low = mid + 1; // In other cases decrease the search space by moving towards right
}
    
```

## Time Complexity

`O(log(n))`

## Space Complexity

`O(1)`

## Solution

```java
public static int lowerBound(int[] nums, int n, int target) {

    // Handle optional n parameter (use array length if not provided)
    if (n <= 0) {
      n = nums.length;
    }

    // Initialize low and high pointers for binary search
    int low = 0;
    int high = n - 1;

    // Loop until the low pointer crosses the high pointer (search space exhausted)
    while (low <= high) {

      // Calculate the middle index to check
      int mid = low + (high - low) / 2;

      // Check if the target is found at the middle index
      if (nums[mid] >= target) {
        // Update potential lower bound and search left half for stricter bound
        result = mid;
        high = mid - 1;
      } else {
        // Search right half for the target
        low = mid + 1;
      }
    }

    // Return the lower bound (either the found index or array length if not found)
    return result;
  }
```
