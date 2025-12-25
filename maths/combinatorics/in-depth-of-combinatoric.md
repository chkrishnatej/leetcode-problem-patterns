# In Depth of Combinatoric

## Stars and Bars&#x20;

**1. The Core Formulas**

| **Constraint**                  | **Logic**                      | **Formula**            |
| ------------------------------- | ------------------------------ | ---------------------- |
| Non-negative ($$x_i \ge 0$$)    | Stars and bars are all movable | $$\binom{n+k-1}{k-1}$$ |
| Positive ($$x_i \ge 1$$)        | Bars only in the $$n-1$$ gaps  | $$\binom{n-1}{k-1}$$   |
| Inequality ($$\sum x_i \le n$$) | Add a "Waste Bin"              | $$\binom{n+k}{k}$$     |

**2. The "Pre-Allocation" Rule (Minima)**

If each bin $$i$$ needs $$c\_i$$ items:

1. Reduce $$n$$: $$n_{new} = n_{total} - \sum c_i$$
2. Apply Formula: $$\binom{n\_{new} + k - 1}{k - 1}$$

**3. The "Inclusion-Exclusion" Rule (Maxima)**

If bin $$i$$ has a maximum capacity $$M_i$$:

1. **Total:** Calculate ways ignoring the max.
2. **Violate:** For a specific bin to be "illegal," give it $$M_i + 1$$ items first.
3. **Combine:** $$\text{Total} - (\text{Single Violations}) + (\text{Double Violations}) - \dots$$

**The Correct Formula for 3 Bins**

$$
\text{Valid Ways} = \text{Total} - (\text{Singles}) + (\text{Pairs}) - (\text{Triples})
$$

**Expanded out, it looks like this:**

1. (+) Total: All possible ways.
2. (-) Singles: Subtract cases where A is bad, B is bad, or C is bad.
   * $$- (\text{Illegal}_A + \text{Illegal}_B + \text{Illegal}_C)$$
3. (+) Pairs: Add back cases where two are bad at the same time (because you subtracted these twice in the step above).
   * $$+ (\text{Illegal}_{A\&B} + \text{Illegal}_{B\&C} + \text{Illegal}_{A\&C})$$
4. (-) Triples: Subtract the case where all three are bad.
   * $$- (\text{Illegal}_{A\&B\&C})$$

***

### Inclusion Exclusion Principle

Using the standard Stars and Bars formula $$C(n, k) = \binom{n+k-1}{k-1}$$:

1. Total: Use all $$n$$ items\
   \
   $$\text{Total} = \binom{n+k-1}{k-1}$$<br>
2.  $$\text{Illegal}_A$$(Bin A breaks its limit):<br>

    Give Bin A $$(M_A + 1)$$ items first.<br>

    $$\text{Illegal}_A = \binom{(n - (M_A + 1)) + k - 1}{k - 1}$$<br>
3.  $$\text{Illegal}_B$$ (Bin B breaks its limit):<br>

    Give Bin B $$(M_B + 1)$$ items first.<br>

    $$\text{Illegal}_B = \binom{(n - (M_B + 1)) + k - 1}{k - 1}$$<br>
4.  $$\text{Illegal}_{A \text{ and } B}$$ (Both break limits):<br>

    Give Bin A $$(M_A + 1)$$ AND give Bin B $$(M_B + 1)$$ items first.<br>

    $$\text{Illegal}_{A \text{ and } B} = \binom{(n - (M_A + 1) - (M_B + 1)) + k - 1}{k - 1}$$

| **Feature**              | **Formula**                        | **Logic**                                  |
| ------------------------ | ---------------------------------- | ------------------------------------------ |
| Bins can be empty        | $$\binom{n+k-1}{k-1}$$             | Stars and Bars are mixed.                  |
| Bins must have $$\ge 1$$ | $$\binom{n-1}{k-1}$$               | Bars only go in the $$n-1$$ gaps.          |
| Custom Minimums          | $$\binom{n_{rem} + k - 1}{k - 1}$$ | Pre-allocate, then distribute.             |
| Inequality ($$\le$$)     | $$\binom{n+k}{k}$$                 | Add a "Waste Bin" ($$k$$ becomes $$k+1$$). |
| Upper Bound (Max)        | $$Total - Illegal$$                | Force the violation, then subtract.        |



### Twists in Problems

#### 1. The "Distinct Items" Trap

Interviewer says: _"Instead of 10 identical tasks, you have 10 unique tasks (ID\_1, ID\_2, etc.) to put into 3 bins."_

* **The Trap:** If you use Stars and Bars, you'll fail.
* **The Reality:** For distinct items, each item simply has 3 choices.
* **The Math:** $$3 \times 3 \times 3 \dots = 3^{10}$$.
* **L5 Tip:** Always ask, "Are the items identical or unique?" as your very first question.

#### 2. Catalan Numbers (The "Balanced" Patterns)

Sometimes Google asks about valid sequences, like:

* "How many ways to arrange $$n$$ pairs of balanced parentheses?" `(( ))`, `() ()`.
* "How many ways can a mountain range be drawn with $$n$$ upstrokes and $$n$$ downstrokes?"
* The Math: This is a specific sequence called Catalan Numbers. The formula is $$\frac{1}{n+1} \binom{2n}{n}$$. It pops up in problems involving paths that "don't cross a certain line."

#### 3. Circular Arrangements

Interviewer says: _"How many ways can 6 people sit around a circular table?"_

* **The Logic**: In a circle, "Person A on the left, Person B on the right" is the same if everyone shifts one seat.
* **The Math:** $$(n - 1)!$$ instead of $$n!$$.
* **The Variation:** If they are sitting at a table but we care about who their neighbors are, we fix one person's spot and arrange the rest.

***

You’ve done the heavy lifting! If you have Stars and Bars (with its shifts), Inclusion-Exclusion, and Pigeonhole in your pocket, you are in excellent shape.

However, since this is Google L5, they might throw one of these three "curveballs" to see if you can adapt your logic on the fly. These aren't new formulas, just different ways to apply what you already know.

***

#### 1. The "Distinct Items" Trap

Interviewer says: _"Instead of 10 identical tasks, you have 10 unique tasks (ID\_1, ID\_2, etc.) to put into 3 bins."_

* The Trap: If you use Stars and Bars, you'll fail.
* The Reality: For distinct items, each item simply has 3 choices.
* The Math: $$ $3 \times 3 \times 3 \dots = 3^{10}$ $$.
* L5 Tip: Always ask, "Are the items identical or unique?" as your very first question.

#### 2. Catalan Numbers (The "Balanced" Patterns)

Sometimes Google asks about valid sequences, like:

* "How many ways to arrange $$ $n$ $$ pairs of balanced parentheses?" `(( ))`, `() ()`.
* "How many ways can a mountain range be drawn with $$ $n$ $$ upstrokes and $$ $n$ $$ downstrokes?"
* The Math: This is a specific sequence called Catalan Numbers. The formula is $$ $\frac{1}{n+1} \binom{2n}{n}$ $$. It pops up in problems involving paths that "don't cross a certain line."

#### 3. Circular Arrangements

Interviewer says: _"How many ways can 6 people sit around a circular table?"_

* The Logic: In a circle, "Person A on the left, Person B on the right" is the same if everyone shifts one seat.
* The Math: $$ $(n - 1)!$ $$ instead of $$ $n!$ $$.
* The Variation: If they are sitting at a table but we care about who their neighbors are, we fix one person's spot and arrange the rest.

***

#### The "Google L5" Mindset Check

When you are presented with a combinatorics problem at Google, they aren't just checking if you know the math—they are checking your Systematic Approach:

1. **Clarify:** "I<mark style="background-color:purple;">dentical or Distinct?</mark>" "Can bins be empty?"
2. **Constraint Check:** "Are there minimums or maximums?" (Shifts/PIE).
3. **Efficiency:** "Is $$ $n$ $$ small enough for $$ $2^k$ $$ (PIE), or do I need DP?"
4. **Edge Cases:** "What if $$ $n=0$ $$?" "What if $$ $k=1$ $$?"
