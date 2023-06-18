# Mountain Array

{% hint style="info" %}
Mountain Array:

> The left side of peak element is in increasing order and right side is in decreasing order


{% endhint %}

### Intuition

A revised binary search based on few invariant conditions.

The **peak** element has property that it is greater on both sides, so we try to check if the `mid` element is largest available in given window. If the condition is satisfied then we return the `max` element in the given window.

Else, we let the loop complete and at the time of completion, the `start` and `end` would be pointing to same index. The given array misses all the conditions we placed in `while` loop for finding `max`, which means, the current index is the `max`

### Implementation

When we don't have `target` to search for, avoid using `<=` in `while` loop

There is a window in which we need to check if the given

{% hint style="info" %}
**mid-1 | mid | mid+1 |**
{% endhint %}

Since the array is rotated, we try to check if the element on the left side is greater

#### Invariant conditions

Check for avoiding **`ArrayOutOfBounds`** and checking if the left side element is greater. If it is greater, then that's the maximum element

```java
if (start < mid && nums[mid - 1] > nums[mid])
    return nums[mid - 1];
if (mid < end && nums[mid] > nums[mid + 1])
     return nums[mid];
```

And if the max element is not found in the iteration, then we need to decrease the search space and check

As we have checked if there are any `max` elements are present on the left side, and they are not found, check towards the right. Here the `nums[start]<=nums[end]` the `<=` the operator is used as the mid has already been checked for if it is the maximum element with the above invariant conditions

```java
if (nums[start] <= nums[mid])
    start = mid + 1;
```

#### Example #1

\[6, 7 , 8, 9, 10, 11, 12, 1, 3, 5]\
`start` = 0 | `end` = 9

```java
if (start < mid && nums[mid - 1] > nums[mid])
    return nums[mid - 1]; // First Invariant
if (mid < end && nums[mid] > nums[mid + 1])
    return nums[mid]; // Second invariant
```

#### First Iteration

`mid`= 4\
`nums[mid]` = `nums[4]` = 10

Checking for invariant conditions

> Checking first invariant condition\
> \* `if(0 < 4 && nums[3] > nums[4]) // False case`\
> \* `if(0 < 4 && 9 > 10) // False case`

> _Checking second invariant condition_

* `if(4 < 9 && nums[4] > nums[5]) // False case`
* `if(4 < 9 && 9 > 10) // False case`

#### Second iteration

`start` = 5 | `end` = 9\
`mid` = 7

Checking for invariant conditions

> Checking first invariant condition\
> \* `if(5 < 7 && nums[6] > nums[7]) // True case`\
> \* `if(5 < 7 && 12 > 1) // True case`

As one of the invariant condition is true, the peak element is found at `mid-1` i.e. `nums[6]` = 12

### Time Complexity

As the search space is decreasing by half at every given iteration, it's $$O(log(n))$$

### Space Complexity

There is no extra space involved so it is $$O(1)$$
