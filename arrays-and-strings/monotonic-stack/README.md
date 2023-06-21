# Monotonic Stack

A monotonic stack is a stack that exhibits the property of elements arranged in a particular sorting order like:

* Strictly increasing (>)
* Non-decreasing (>=)
* Strictly decreasing (<)
* Non-increasing (<=)

{% content-ref url="../../concepts/arrays-and-their-sorted-orders.md" %}
[arrays-and-their-sorted-orders.md](../../concepts/arrays-and-their-sorted-orders.md)
{% endcontent-ref %}

The general template of the algorithm looks like this:

```java
public class Solution {
    public static void main(String[] args) {
        int[] nums = new int[]{1,2,3,4,5};
        Stack<Integer> stack = new Stack<>();
        
        // Iterating using indexes as we might store indexes rather than values
        for(int i=0;i<nums.length;i++) {
            // Here operator can be filled according to the given condition
            // Strictly increasing (>)
            // Non-decreasing (>=)
            // Strictly decreasing (<)
            // Non-increasing (<=)
            while(!stack.isEmpty() && stack.peek() `OPERATOR` nums[i]) {
                int stackTop = stack.pop();
                
                 // do something with stackTop here e.g.
                // nextGreater[stackTop] = i
            }
            if(!stack.isEmpty()) {
                // if stack has some elements left
               // do something with stack top here e.g.
              // previousGreater[i] = stack.peek()
            }
            stack.push(i);
        }
        
    }
}
    
```

**Notes about the template above**

* We initialize an empty stack at the beginning.
* The stack contains the index of items in the array, not the items themselves
* There is an outer `for` loop and inner `while` loop.
* At the beginning of the program, the stack is empty, so we don't enter the `while` loop at first.
* The earliest we can enter the `while` loop body is during the second iteration of `for` loop. That's when there is at least an item in the stack.
* At the end of the `while` loop, the index of the current element is pushed into the stack
* The `OPERATOR` inside the while loop condition decides what type of monotonic stack are we creating.
* The `OPERATOR` could be any of the four - `>, >=, <, <=`

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

## Monotonic Stack Types

| Problem          | Stack Type                 | Operator in while loop | Assignment Position |
| ---------------- | -------------------------- | ---------------------- | ------------------- |
| next greater     | decreasing (equal allowed) | stackTop < current     | inside while loop   |
| previous greater | decreasing (strict)        | stackTop <= current    | outside while loop  |
| next smaller     | increasing (equal allowed) | stackTop > current     | inside while loop   |
| previous smaller | increasing (strict)        | stackTop >= current    | outside while loop  |

{% tabs %}
{% tab title="StrictlyIncreasingStack" %}
```java
class Solution {
    public int[] getMonotonicStrictlyIncreasingStack(int[] input) {
        int n = input.length;
        int[] stack = new int[n];
        int top = -1; // Stack top pointer

        for (int i = 0; i < n; i++) {
            // When you encounter a number from input which is greater than stack top
            // Keep popping elements from stack that are greater than current element
            while (top >= 0 && stack[top] > input[i]) {
                top--; // Pop elements greater than current element from stack
            }
            stack[++top] = input[i]; // Push current element onto stack
        }

        // Create a new array with elements from the stack
        int[] result = new int[top + 1];
        System.arraycopy(stack, 0, result, 0, top + 1);

        return result;
    }
}

```
{% endtab %}

{% tab title="NonDecreasingStack" %}
```java
class Solution {
    public int[] getMonotonicNonDecreasingStack(int[] input) {
        int n = input.length;
        int[] stack = new int[n];
        int top = -1; // Stack top pointer

        for (int i = 0; i < n; i++) {
            // When you encounter a number from input which is greater than or equal
            // stack top
            // Keep popping elements from stack that are greater than or equal
            // than current element
            while (top >= 0 && stack[top] >= input[i]) {
                top--; // Pop elements greater than current element from stack
            }
            stack[++top] = input[i]; // Push current element onto stack
        }

        // Create a new array with elements from the stack
        int[] result = new int[top + 1];
        System.arraycopy(stack, 0, result, 0, top + 1);

        return result;
    }
}

```
{% endtab %}

{% tab title="StrictlyDecreasingStack" %}
```java
class Solution {
    public int[] getMonotonicStrictlyDecreasingStack(int[] input) {
        int n = input.length;
        int[] stack = new int[n];
        int top = -1; // Stack top pointer

        for (int i = 0; i < n; i++) {
            // When you encounter a number from input which is lesser than stack top
            // Keep popping elements from stack that are lesser than current element
            while (top >= 0 && stack[top] < input[i]) {
                top--; // Pop elements greater than current element from stack
            }
            stack[++top] = input[i]; // Push current element onto stack
        }

        // Create a new array with elements from the stack
        int[] result = new int[top + 1];
        System.arraycopy(stack, 0, result, 0, top + 1);

        return result;
    }
}

```
{% endtab %}

{% tab title="NonIncreasingStack" %}
```java
class Solution {
    public int[] getMonotonicNonIncreasingStack(int[] input) {
        int n = input.length;
        int[] stack = new int[n];
        int top = -1; // Stack top pointer

        for (int i = 0; i < n; i++) {
            // When you encounter a number from input which is lesser than or equal
            // stack top
            // Keep popping elements from stack that are less or equal than current element
            while (top >= 0 && stack[top] < input[i]) {
                top--; // Pop elements greater than current element from stack
            }
            stack[++top] = input[i]; // Push current element onto stack
        }

        // Create a new array with elements from the stack
        int[] result = new int[top + 1];
        System.arraycopy(stack, 0, result, 0, top + 1);

        return result;
    }
}

```
{% endtab %}
{% endtabs %}

## Time Complexity

<table><thead><tr><th width="120">Operation</th><th width="67">Best</th><th width="70">Worst</th><th width="94">Average</th><th>Explanation</th></tr></thead><tbody><tr><td>Push</td><td>O(1)</td><td>O(1)</td><td>O(1)</td><td>Pushing an element onto the stack takes constant time.</td></tr><tr><td>Pop</td><td>O(1)</td><td>O(1)</td><td>O(1)</td><td>Popping an element from the stack takes constant time.</td></tr><tr><td>Top</td><td>O(1)</td><td>O(1)</td><td>O(1)</td><td>Retrieving the top element of the stack takes constant time.</td></tr><tr><td>Finding Next Greater/Smaller Element</td><td>O(1)</td><td>O(n)</td><td>O(n)</td><td>Finding the next greater/smaller element for each element in the stack can have a best case of constant time when the elements are in non-decreasing/non-increasing order. However, in the worst case, it may require traversing the entire stack, resulting in a linear time complexity.</td></tr><tr><td>Calculating Nearest Smaller/Larger Elements</td><td>O(1)</td><td>O(n)</td><td>O(n)</td><td>Calculating the nearest smaller/larger elements for each element in the stack can have a best case of constant time when the elements are in non-decreasing/non-increasing order. However, in the worst case, it may require traversing the entire stack, resulting in a linear time complexity.</td></tr></tbody></table>

## Space Complexity

<table><thead><tr><th width="125">Operation</th><th width="77">Best</th><th width="85">Worst</th><th width="100">Average</th><th>Explanation</th></tr></thead><tbody><tr><td>Push</td><td>O(1)</td><td>O(1)</td><td>O(1)</td><td>Pushing an element onto the stack takes constant space.</td></tr><tr><td>Pop</td><td>O(1)</td><td>O(1)</td><td>O(1)</td><td>Popping an element from the stack takes constant space.</td></tr><tr><td>Top</td><td>O(1)</td><td>O(1)</td><td>O(1)</td><td>Retrieving the top element of the stack takes constant space.</td></tr><tr><td>Finding Next Greater/Smaller Element</td><td>O(n)</td><td>O(n)</td><td>O(n)</td><td>Finding the next greater/smaller element for each element in the stack requires storing the result for each element, resulting in a space complexity linear to the size of the stack (O(n)).</td></tr><tr><td>Calculating Nearest Smaller/Larger Elements</td><td>O(n)</td><td>O(n)</td><td>O(n)</td><td>Calculating the nearest smaller/larger elements for each element in the stack requires storing the result for each element, resulting in a space complexity linear to the size of the stack (O(n)).</td></tr></tbody></table>

### Problems

* Basic: [496-Next Greater Element I](https://x-czh.github.io/Algorithms-LeetCode/400-499/496-Next-Greater-Element-I.cpp) and [1019-Next Greater Node In Linked List](https://x-czh.github.io/Algorithms-LeetCode/1000-1099/1019-Next-Greater-Node-In-Linked-List.html).
* Dealing with circular list: [503-Next Greater Element II](https://x-czh.github.io/Algorithms-LeetCode/500-599/503-Next-Greater-Element-II.cpp).
* Variant: [042-Trapping Rain Water](https://x-czh.github.io/Algorithms-LeetCode/000-099/049-Trapping-Rain-Water-.md).
* Variant: [084-Largest Rectangle in Histogram](https://x-czh.github.io/Algorithms-LeetCode/000-099/084-Largest-Rectangle-in-Histogram.html).

### References:

* [A comprehensive guide and template for monotonic stack based problems - LeetCode Discuss](https://leetcode.com/discuss/study-guide/2347639/A-comprehensive-guide-and-template-for-monotonic-stack-based-problems)
* [Algorithms for Interview 2: Monotonic Stack | by Yang Zhou | TechToFreedom | Medium](https://medium.com/techtofreedom/algorithms-for-interview-2-monotonic-stack-462251689da8)

