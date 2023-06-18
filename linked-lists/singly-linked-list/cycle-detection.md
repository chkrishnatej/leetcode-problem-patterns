# Cycle Detection

Assume that we have a linked list as shown in the below image.

<figure><img src="https://i1.wp.com/codinggladiators.com/wp-content/uploads/2021/07/fcd_details2.png?resize=512%2C273" alt="" height="273" width="512"><figcaption></figcaption></figure>

Here we will assume that this algorithm works and we will work our way back to prove this assumption. For this, we will assume the following parameters.

$$m$$ = the distance from the head node to the starting node of the cycle\
$$k$$ = the distance from the starting node of the cycle to the matched node\
$$c$$ = the length of the cycle i.e. the number of nodes in the cycle

> The distance traveled by the tortoise node to reach the node at which it meets with the hare node can be represented as: (m + k)

> The distance traveled by the hare node to reach the node at which it meets with the tortoise node can be represented as:



Here, � is the number of rotations the hare pointer does in the loop before meeting the tortoise pointer. In our case, the hare pointer does only one rotation. But in other cases, it may do more than one rotation before meeting the tortoise pointer.

As we already know that the tortoise pointer is moving one node at a time and the hare pointer is moving two nodes at a time, when they both meet we can safely conclude that the hare pointer has covered twice the distance as the tortoise pointer. Therefore,

�+�+��=2(�+�)

After simplifying the equation we get,

�+�+��=2(�+�)�+�+��=2�+2���=2�–�+2�–���=�+��+�=��

Thus the final equation that we get is

�+�=��

Let’s focus on the right side of the equation i.e. �� and what it tells us. Here � is the number of rotations the hare pointer does before meeting the tortoise pointer and � is the length of the loop.
