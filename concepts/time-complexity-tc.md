---
icon: clock
---

# Time Complexity(TC)

### TC while counting digits of a number

`O(log10(N))` ➔   In every iteration we are dividing N by 10 (equivalent to the number of digits in N).

$$log_{10} n$$ is the standard mathematical notation for the number of digits/iterations, where $$n$$ is the value of the input number.

But in interview, be careful, tell both ways

1. When $$n$$ is the number , since we are dividing the number by 10 at every step the time complexity follows a logarithmic scale, then the TC is $$log_{10}n$$
2. When $$D$$ is considered as number of digits, then the time complexity can be represented as $$O(D)$$
