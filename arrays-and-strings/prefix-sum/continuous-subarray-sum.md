# Continuous Subarray Sum

### Problem

[`https://leetcode.com/problems/continuous-subarray-sum/`](https://leetcode.com/problems/continuous-subarray-sum/)

<figure><img src="../../.gitbook/assets/image (25) (1).png" alt=""><figcaption></figcaption></figure>

### Understanding of problem

The continuous subarray sum should be multiple of `k` , and the length of the subarray should be at least 2.

For example, if the `k` is **6,** this is how the preSum and `preSum%k` looks like.&#x20;

> The reason for taking the modulo is beacuase we are trying to find the multiples. And the base concept is if modulo of number is zero, the number is a multiple.&#x20;

As we try to find the contiguous sub-arrays, we can use the principle of prefix sum.

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

Store the `preSum%k` in HashMap with its position for easy lookup

{% code lineNumbers="true" %}
```
sum1 = a = 23
sum2 = a + b + c = 29
```
{% endcode %}

And using the **modulo congruence**,&#x20;

$$
(sum2 - sum1)\%k = 0 \\ sum2\%k - sum1\%k = 0 \\ sum2\%k = sum1\%k
$$

{% code lineNumbers="true" %}
```
(29 - 23)%6 = 0, cause 6%6 = 0
29%6 - 23%6 = 0
29%6 = 23%6
```
{% endcode %}

This means whenever we encounter the same modulo, using the above modulo congruence relation, the subarray sum is multiple of `k` .

### Edge case

The below case is an edge case that needs to be handled.

> `k` = 7. We are trying to find multiples of 7

<figure><img src="../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

When we see the modulo as zero, even before we find a similar modulo, we need to check the length of the subarray.

In the above case, we have the subarray sum of `a+b` = 21. 21 is a multiple of 7. Since the problem asks to check if the length of the subarray should be at least 2, which(length) is calculated based on the values stored in the HashMap, even though it is a valid subarray, it is not counted as one and gives out false.

To handle this kind of case, we add

```java
map.put(0, -1);
```

Putting `-1` will make sure the above case is not handled correctly.&#x20;

If we calculate now,&#x20;

<pre><code>current i = 1
length of subarray = i - map.get(0) 

<strong>length of subarray = 1 - (-1) = 1 + 1 = 2
</strong></code></pre>

### Solution

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        // n -> Length of the input array
        // Time complexity: O(n)
        // Space complexity: O(n), Space taken by hashmap
        
        // Handling edge case, where the input length is itself less than 2
        if(nums.length < 2) {
            return false;
        }

        // Stores the modulo and its position
        Map<Integer, Integer> map = new HashMap<>();
        /*
        Edge case to handle first zero modulo 
        This is to handle edge case. If the mod value is 0 on first index
        i.e. zeroth index then the length of sub array is < 2.
        If it is on first index, then if we calculate the distance
            i = 1, map.get(0) = -1
            i-map.get(0) = 1 - (-1) = 2;
        */
        map.put(0,-1);
        int prefix = 0; // Prefix sum

        for(int i=0;i<nums.length;i++) {
            prefix += nums[i];

            // Modulo of the prefix
            int complement = prefix % k;

            // If modulo exists and length of sub-array is greater than or equal to 2
            if(map.containsKey(complement) && i - map.get(complement) >= 2) {
                return true; // Found good sub array
            } 
            // Using putIfAbsent as we are trying to find the 
            // length of good subarrays which has the constraint
            // it should be of length 2
            // And we are storing complements as we are using modulo congruence 
            // constraint
            map.putIfAbsent(complement, i);
        }
        return false;
    }
}
```
