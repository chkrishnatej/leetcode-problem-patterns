# Prefix Sum

### Concept

<mark style="background-color:green;">The array sum at every given index is pre-calculate by iterating through the array</mark>

{% hint style="info" %}
Normal Array --> \[1  2  3  4  5]

Prefix Array  -->  \[1  3  6  10  15]
{% endhint %}

### Prefix operations

* `A[]` → Normal Array
* `B[]` → Prefix Array

{% hint style="info" %}
```
Initial → preSum[0] = nums[0]
```

```
preSum[i] = preSum[i-1] + A[i], where i > 0
```
{% endhint %}

This approach works because, when storing the prefix sums, we have the sum available at that point.&#x20;

$$
sum[i,j] = sum[0,j] -sum[0, i-1], where\:i < j
$$

{% hint style="info" %}
&#x20;                             \<target> = \<sum till `j`> - \<sum till `i`>
{% endhint %}

The reason for subtracting is since we are maintaining a prefix sum, we already have the sum of the sub-array till the point `j`. The point `i` contains the sum till&#x20;

<figure><img src="../../.gitbook/assets/image (22) (1).png" alt=""><figcaption><p>Full Array with presum</p></figcaption></figure>

Let's consider in the above table, we want to get the sum of the subarray from \[1,2]

<figure><img src="../../.gitbook/assets/image (27) (1).png" alt=""><figcaption><p>Calculating subarray sum from indices [1,2]</p></figcaption></figure>

Looking at it, we can understand that the sum of the given subarray is 5. How we can derive it in the following way<br>

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Explaination of calculation</p></figcaption></figure>

<mark style="background-color:green;">So in the code, when we find the complement in the</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">`preSum`</mark><mark style="background-color:green;">, it means that a</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">**ranged query/subarray contains the required target.**</mark>

### Type of problems

This technique can be applied in cases of **arrays** mainly. <mark style="background-color:yellow;">The Prefix Sum technique can be used when the sliding window does not seem to work, like the problem does not get a proper condition to shrink or expand the window.</mark>

* Sub array problems
* Ranged sums
* Equilibrium index → Sum of left side == sum of right side
* Subarray with 0 sum
* Maximum subarray size, such that all subarrays of that size have sum less than k
* Maximum occurred integer in n range
* **Exactly "k" number of integers = atmost(k)-atmost(k-1)**&#x20;

#### Practice Problems:

* [https://leetcode.com/list/epvy5cd2](https://leetcode.com/list/epvy5cd2)
* [Prefix Sum Problems - LeetCode Discuss](https://leetcode.com/discuss/general-discussion/563022/prefix-sum-problems)

#### References:

* [Prefix Sum Array - Implementation and Applications in Competitive Programming - GeeksforGeeks](https://www.geeksforgeeks.org/prefix-sum-array-implementation-applications-competitive-programming/)
* [C++| Full explained every step w/ Dry run | O(n^2) -> O(n) | Two approaches ](https://leetcode.com/problems/subarray-sum-equals-k/discuss/1759909/C%2B%2Bor-Full-explained-every-step-w-Dry-run-or-O\(n2\)-greater-O\(n\)-or-Two-approaches)→ Clear cut explanation
* [General summary of what kind of problem can/ cannot solved by Two Pointers](https://leetcode.com/problems/subarray-sum-equals-k/discuss/301242/General-summary-of-what-kind-of-problem-can-cannot-solved-by-Two-Pointers)
* [https://leetcode.com/problems/subarrays-with-k-different-integers/solutions/235235/C++Java-with-picture-prefixed-sliding-window/](https://leetcode.com/problems/subarrays-with-k-different-integers/solutions/235235/C++Java-with-picture-prefixed-sliding-window/)
