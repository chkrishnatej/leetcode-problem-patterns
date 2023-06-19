# Find First and Last Position of Element in Sorted Array

### Problem

[https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

<figure><img src="../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

### Solution

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        // n -> Length of nums
        // Time complexity: O(log n)
        // Space complexity: O(1), auxillary space complexity
        int firstPos = binarySearchInRange(nums, target, true);
        int lastPos = binarySearchInRange(nums, target, false);

        return new int[]{firstPos, lastPos};
    }

    private int binarySearchInRange(int[] nums, int target, boolean isFirst) {
        int start = 0, end = nums.length - 1;
        int result = -1;

        // Standard Binary search
        while(start <= end) {
            int mid = start + (end-start)/2;

            // When we find the target, we want to find its 
            // first and last occurence, which is controlled
            // by the flag `isFirst`
            if(nums[mid] == target) {
                result = mid;
                // If checking for first occurrence, search towards left.
                // As it is non decreasing list
                if(isFirst) {
                    end = mid - 1;
                } 
                // Else, look for last occurence, search towards right
                else {
                    start = mid + 1;
                }
            }
            else if(nums[mid] < target) {
                start = mid + 1;
            } else if(target < nums[mid]) {
                end = mid - 1;
            }
        }
        return result;
    }
}
```
