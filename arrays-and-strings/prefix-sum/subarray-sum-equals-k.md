# 🆗 Subarray sum equals k

### Problem

[https://leetcode.com/problems/subarray-sum-equals-k/description/](https://leetcode.com/problems/subarray-sum-equals-k/description/)

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### Understanding of problem

### Solution&#x20;

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        // n -> length of array "n"
        // Time complexity: O(n) 
        // It is one pass
        // Space complexity: O(n) (for Hashmap)


        // Keeps track of {sum till index: number of times it occurred}
        Map<Integer, Integer> map = new HashMap<>();
        // When we encounter "k" in the range from [0, i],
        // It is also a valid subarray

        // Since we are considering complements,
        // If the complement of [0,i] == 0, then the subarray
        // [0, i] should be counted. It is an edge case, so we add
        // map.put(0,1), to cover that case
        map.put(0,1); 

        int prefix = 0, count = 0;

        for(int i=0;i<nums.length;i++) {
            prefix += nums[i]; // Prefix sum, i.e sum so far
            int complement = prefix - k;
            // Checking if the complement is present in the map, 
            // helps in understanding if a 
            // particular range in array contains sum == "k"
            // If complement is present in the map, then increment the count
            count += map.getOrDefault(complement, 0);

            // Store the prefix sum with the number of times it occurred
            // It might not make sense with only positive integers,
            // But when you have negative numbers, then a particular prefix might
            // be occurring more than once
            map.put(prefix, map.getOrDefault(prefix,0)+1);
        }
        return count;
    }
}
```

### Reference Submissions

* [https://leetcode.com/problems/subarray-sum-equals-k/submissions/913837394/](https://leetcode.com/problems/subarray-sum-equals-k/submissions/913837394/)
* [https://leetcode.com/problems/subarray-sum-equals-k/submissions/734216658/](https://leetcode.com/problems/subarray-sum-equals-k/submissions/734216658/)
