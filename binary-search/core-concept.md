# Core Concept

### Types of problems

* Regular ones
* Peak Element
* What is the lowest occurrence / highest occurrence
* Searching in a sorted 2D array

### Code for ascending binary search

```java
class Solution {
    public int search(int[] nums, int target) {
        // Time complexity: O(log n), n -> length of input array
        // log n because search space is halved at each iteration
        // Space complexity: O(1), auxiliary space
        int start = 0, end = nums.length - 1;

        // Initiate iteration
        while(start <= end) {
            int mid = start + (end-start)/2; // Get the mid index
            
            // In this case, the nums[mid] == target, so we found the index                  
            if(nums[mid] == target) {
                return mid;
            }
            // If num[mid] is less than the target, we are looking out for a number greater than nums[mid]
            // Since the array is sorted in ascending order, search towards the right 
            else if(nums[mid] < target) {
                start = mid + 1;
            } 
            // If target is less than nums[mid], we are looking out for number lesser than nums[mid]
            // Subce array is sorted in ascending, search towards left
            else if(target > nums[mid]) {
                end = mid - 1;
            }
        }
        // If code flow has come here, it means the target is not present in the array
        return -1; 
    }
}
```



### Why use \`start<=end\` and not \`start < end\`?

{% embed url="https://stackoverflow.com/a/44231540" %}

If you replace `start <= end` it by `start < end`, there will be cases where your algorithm will not give a good answer.

Let's think about two cases.

* You would like to **search for 1** in the list `[1]`. In that case, `start = 0, end = 0` the **algorithm would return -1** if you change the loop condition to `start < end`.
* You would like to **search for 2** in the list `[1, 2]`. In that case, `start = 0, end = 1`. The algorithm will be set `mid=(0+1)/2=0` in C. Thus `arr[mid] < key`. It will make `start = 1, end = 1`. Again, if you stop the loop here, the algorithm **will return -1 instead of 1.**

### Calculating Mid

$$
\boldsymbol {{Original\ equation}} \\ \\
mid = \frac{start+end}2\\ \\
$$

$$
\boldsymbol {{Better\ equation}} \\ \\
mid = start\ + \dfrac{(end-start)}2  \\ \\
$$

$$
mid = \dfrac{2*start + end - start}2 \\ \\
In\ the\ above\ equation\ if\ we\ simplify,\ (2*start - start) = start  \\ \\
$$

$$
Therefore,\ \boldsymbol {{mid = \dfrac{start + end}2}}
$$

### References:

* [<mark style="background-color:purple;">An opinionated guide to binary search (comprehensive resource with a bulletproof template)</mark> ](https://leetcode.com/discuss/study-guide/2371234/An-opinionated-guide-to-binary-search-\(comprehensive-resource-with-a-bulletproof-template\)) --> Stuff of legends

{% embed url="https://youtu.be/W9QJ8HaRvJQ" %}

* [Come on, forget the binary search pattern/template! Try understand it! - Search Insert Position - LeetCode](https://leetcode.com/problems/search-insert-position/solutions/249092/come-on-forget-the-binary-search-pattern-template-try-understand-it/?orderBy=most\_votes)
* [Find position of an element in a sorted array of infinite numbers - GeeksforGeeks](https://www.geeksforgeeks.org/find-position-element-sorted-array-infinite-numbers/#discuss)
* [✅ A GUIDE TO THE BINARY SEARCH ALGORITHM!!! - LeetCode Discuss](https://leetcode.com/discuss/study-guide/1233854/a-noobs-guide-to-the-binary-search-algorithm)
  * The above guide contains 3 commonly asked types of binary search
  * Regular Binary Search
  * Finding right or leftmost occurrence in a sorted array
* [Binary Search for Beginners \[Problems | Patterns | Sample solutions\] - LeetCode Discuss](https://leetcode.com/discuss/study-guide/691825/Binary-Search-for-Beginners-Problems-or-Patterns-or-Sample-solutions)
* [\[Python\] Powerful Ultimate Binary Search Template. Solved many problems - LeetCode Discuss](https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems)
* [https://leetcode.com/discuss/study-guide/2092413/Binary-Search-Cheatsheet-oror-All-Templates-are-Covered](https://leetcode.com/discuss/study-guide/2092413/Binary-Search-Cheatsheet-oror-All-Templates-are-Covered)
* [Find the position of an element in a sorted infinite array | CalliCoder](https://www.callicoder.com/search-in-sorted-infinite-array/)
