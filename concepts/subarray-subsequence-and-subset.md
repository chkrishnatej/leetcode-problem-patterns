# Subarray, Subsequence and Subset

| Type         | Contigious | Order             | Number of possibilities |
| ------------ | ---------- | ----------------- | ----------------------- |
| Sub Array    | Yes        | Yes               | $$\dfrac{n*(n+1)}2$$    |
| Sub Sequence | No         | Relative Ordering | $$2^n-1$$               |
| SubSet       | No         | No                | $$2^n$$                 |

> * Every Subarray is a Subsequence.
> * Every Subsequence is a Subset.

### Original Array

```markup
Original array - {1, 2, 3, 4}
```

### Sub Array

<mark style="background-color:green;">A subarray is a contiguous part of an array that maintains elements' relative ordering.</mark>

```markup
Sub array - {2, 3}
```

$$
Total\ number\ of\ sub arrays = \dfrac{n*(n+1)}2
$$

### Subsequence

<mark style="background-color:green;">A subsequence is a</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">**sequence of elements that maintains the order of elements**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">that might not be contiguous. Subsequences do not contain empty sets.</mark>

```
Subsequence - {1, 3}
```

$$
Total\ number\ of\ subsequences\ = 2^n-1, without\ empty\ subsequence
$$

###

### Subset

<mark style="background-color:green;">A subset</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">**does not maintain a relative ordering of elements**</mark><mark style="background-color:green;">, nor is a contiguous part of an array. Subsets can have empty sequences.</mark>

```
Subset - {3, 2}
```

$$
Total\ number\ of\ subsets = 2^n
$$

We can answer that by understanding the **difference between Subarray, Subsequence and Subset**

* A **subarray is a contiguous part of array and maintains relative ordering of elements**. For an array/string of size n, there are n\*(n+1)/2 non-empty subarrays/substrings.
* A **subsequence maintain relative ordering of elements but may or may not be a contiguous part of an array**. For a sequence of size n, we can have 2^n-1 non-empty sub-sequences in total.
* A **subset MAY NOT maintain relative ordering of elements and can or cannot be a contiguous part of an array**. For a set of size n, we can have (2^n) sub-sets in total.

Let us understand it with an example.

Consider an array:

array = \[1,2,3,4]

* Subarray : \[1,2],\[1,2,3] - is continous and maintains relative order of elements
* Subsequence: \[1,2,4] - is not continous but maintains relative order of elements
* Subset: \[1,3,2] - is not continous and does not maintain relative order of elements
