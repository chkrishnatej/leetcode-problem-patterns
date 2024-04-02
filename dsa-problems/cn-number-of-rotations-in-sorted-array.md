---
description: '#binary-search'
---

# CN: Number of rotations in sorted array

## Problem

{% embed url="https://www.naukri.com/code360/problems/rotation_7449070" %}

## Intuition

It is the same as finding the minimum value in rotated sorted array. [153.-find-minimum-in-rotated-sorted-array.md](153.-find-minimum-in-rotated-sorted-array.md "mention").

The number of rotations is the index in which minimum value is present.

## Time Complexity

`O(log n)`

## Space Complexity

`O(1)`

## Solution

```java
public class Solution {
    public static int findKRotation(int []nums){
        // Write your code here.
        int low = 0, high = nums.length-1;

        int rotations = 0;
        int min = Integer.MAX_VALUE;

        while(low <= high) {
            int mid = low + (high-low)/2;

            if(nums[low] <= nums[mid]) { // Left sorted
                // Check if a new minimum is found and updated the min value
                if(nums[low] < min) {
                    min = nums[low];
                    rotations = low; // Assign the current rotations to low in left sorted
                }
                low = mid + 1;
            } else { // Right sorted
                // Check if new minimum is found and update the min value
                if(nums[mid] < min) {
                    min = nums[mid]; 
                    rotations = mid; // Assign the current rotations to mid in right sorted
                } 
                high = mid -1;
            }
        }

        return rotations;
    }
}
```
