# Modulo Congurence

This is helpful in solving problems like rotation of arrays.

Given  `k` , which represents number of rotations that needs to be made on an array, we can use modulo congurence to achieve it<br>

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

***

### Normalization&#x20;

Here are the single-line reasons for each:

1. **Normalization**: It prevents "**Out of Bounds**" errors and eliminates redundant full-circle movements by reducing any shift $$k$$ to its effective value within the range of the array size $$n$$.
2. **Double Modulo**: It forces Java's remainder-based `%` operator to return a mathematically correct positive index, even when $$k$$ is negative (Left Shift).

***

| **Task**                  | **Formula for k**                                  |
| ------------------------- | -------------------------------------------------- |
| Normalize Right Shift     | `k = (k % n + n) % n;`                             |
| Normalize Left Shift      | `k = ((-k) % n + n) % n;`                          |
| Find Target Index (Right) | `(oldIndex + k) % n` (after normalizing $$k$$)     |
| Find Target Index (Left)  | `(oldIndex - k + n) % n` (after normalizing $$k$$) |
