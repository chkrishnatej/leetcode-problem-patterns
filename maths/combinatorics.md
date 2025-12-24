# Combinatorics

{% embed url="https://www.youtube.com/watch?v=VJkvPTY6kZw" %}

## Basic Counting

If there are `m` outcomes for event `A` and `n` outcomes for event `B`, then the total number of outcomes for both event `A` and `B` are:

$$
totalNumberOfOutcomes = m * n
$$

And if there are more events like `o` outcomes for event `C` then we simply need to multiply to get the total number of outcomes

$$
totalNumberOfOutcomes = m * n * 0
$$

***

## Permutations

A way in which a set of things can be ordered



<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

When there are duplicate elements and we are tasked to find the different permutations, then the total number of elements factorial is taken and is divided by all the possible factorials of each unique element.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

***

{% hint style="info" %}
The major difference between permutation and combination is \
1\. Permutation is arrangement (**ORDER matters**)\
2\. Combination is selection (**ORDER DOES NOT MATTER**)
{% endhint %}

***

## Combinations

The number of groups of `r` objects that can be formed from total of `n` objects

{% hint style="info" %}
Ordering does not matter. Even if the group has different order but the same objects, it is still considered to be same
{% endhint %}

Its formula is represented by&#x20;

$$
C(n, k) = \binom{n}{k} = \frac{n!}{k!(n-k)!}
$$

* $$n$$ is the total number of items in the set.
* $$k$$ is the number of items to be selected.

When you are explaining this in a Google interview, break the formula down into its two logical parts:

1. $$\frac{n!}{(n-k)!}$$: This is the formula for Permutations. It calculates all possible ordered arrangements.
2. $$\frac{1}{k!}$$: This is the "**Deduplication**" factor. Since the order doesn't matter in combinations, you divide by $$k!$$ to remove all the different orderings of the same group of items.

***

## Partitions

Dividing a set of elements into subsets

Dividing elements into groups

There are two different types of partitions

### Dividing Distinct elements into groups

This type of problem involves determining how many ways you can group a set of distinct elements into distinct groups of specified sizes.

**Example (Fish Problem):** Imagine there are eight different fish, and three people (Jiggle Bob, Max, and Reagan) want to eat a specific number of fish: Jiggle Bob wants four fish, Max wants two, and Reagan wants two (19:47-19:58). The question is how many ways these eight different fish can be grouped into distinct groups of four, two, and two (19:58-20:06).

### Stars and Bars method

This technique can be used in distributed problems.

#### 1. The Core Concept

Use this when distributing $$n$$ identical items into $$k$$ distinct bins.

* **Stars (**$$\star$$**):** The items you are distributing.
* **Bars (**$$|$$**):** The dividers that create the bins. <mark style="background-color:$primary;">To make</mark> $$k$$ <mark style="background-color:$primary;">bins, you need</mark> $$k-1$$ <mark style="background-color:$primary;">bars.</mark>

#### 2. The Two Standard Scenarios

**Scenario A: Bins can be Empty (Non-negative)**

* **Problem**: $$x_1 + x_2 + \dots + x_k = n$$, where $$x_i \ge 0$$.
* **Logic**: You have $$n$$ stars and $$k-1$$ bars. You are choosing positions for the bars out of a total of $$(n + k - 1)$$ spots.
* **Formula**:&#x20;

$$
\binom{n + k - 1}{k - 1}
$$

**Scenario B: Bins MUST have at least 1 (Positive) (No Empty bins)**

* **Problem**: $$x_1 + x_2 + \dots + x_k = n$$, where $$x_i \ge 1$$.
* **Logic**: You place the bars in the "gaps" between stars. There are $$n-1$$ gaps, and you need to pick $$k-1$$ of them.
* **Formula**:&#x20;

$$
\binom{n - 1}{k - 1}
$$

$$
\star \underbrace{\downarrow}_{\text{Gap 1}} \star \underbrace{\downarrow}_{\text{Gap 2}} \star \underbrace{\downarrow}_{\text{Gap 3}} \star
$$

* In this case, there is a constraint that each bin but have atleast one element. So we need to elimenate the illegal positions and only consider the legal positions.
* The legal positions for the bars are the gaps as shown above. If number of stars are `n` then the legal positions in which the bars can be placed are `n-1` jd
  * Illegal Positions of bars:
    * At the very ends
    * Adjacent to each other
* So now, we need to select the positions in which we can place the bars which is a function of\
  $$\binom{n-1}{k-1}$$



#### Twist 1: The "Custom Minimums" (Reduction)

**Scenario:** Each bin $$i$$ must have at least $$c_i$$ items. (e.g., $$x_1 \ge 2, x_2 \ge 5$$).

**The Logic:**

Think of this as a pre-allocation. Before you start "choosing" positions, you must "pay" the minimums.

1. You take the required items out of the bag and put them in the bins.
2. The items left in your bag ($$n - \sum c_i$$) are now "free stars."
3. Because the "contract" is already satisfied for every bin, these remaining stars can be distributed however you like—including giving zero to a bin.

**The Pool:** You are now in Scenario A with a smaller number of stars.

**Formula:**&#x20;

$$
\binom{(n - \text{pre-allocated}) + k - 1}{k - 1}
$$

#### Twist 2: The "Inequality" (The Dummy Bin)

**Scenario:** $$x_1 + x_2 + x_3 \le 10$$. (The items don't _have_ to be used up).

**The Logic:**

In standard Stars and Bars, you must use all $$n$$ stars. To handle an "up to $$n$$" problem, we create a "Waste Bin" (or Dummy Bin).

* Any star you don't want to give to $$x_1, x_2, \text{ or } x_3$$, you throw into the Waste Bin ($$x_4$$).
* The equation becomes: $$x_1 + x_2 + x_3 + x_{waste} = 10$$.

**The Pool:** You haven't changed the number of stars $$(n)$$, but you have added one extra bin $$(k+1)$$

**Formula:**&#x20;

$$
\binom{n + (k+1) - 1}{(k+1) - 1} \rightarrow \binom{n+k}{k}
$$

| **Twist**  | **The Logic**           | **The Shift**           |
| ---------- | ----------------------- | ----------------------- |
| Minimums   | Pre-allocate items      | $$n$$ decreases         |
| Inequality | Add a "Waste Bin"       | $$k$$ increases         |
| Maximums   | Subtract illegal states | Use Inclusion-Exclusion |
