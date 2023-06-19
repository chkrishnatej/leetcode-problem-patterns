# Search Insert Position

### Problem

<figure><img src="../../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

### Solution

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        // n -> Length of nums
        // Time complexity: O(log n)
        // Space complexity: O(1) -> Auxillary space
        int start = 0, end = nums.length-1;

        // Standard binary search
        while(start <= end) {
            int mid = start + (end-start)/2;

            if(nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                start = mid + 1;
            } else if(target < nums[mid]) {
                end = mid - 1;
            }
        }
        // when the target is not found, it means that the condition
        // start <= end is violated. That means answer does not lie 
        // between start and end. 
        // And when the condition is violated, the start pointer will land
        // up at the next greater element, because start is ahead of end
        // And start will land up exactly
        // at next smallest greater number possible where we need to
        // insert the target;
        return start;
    }
}
```
