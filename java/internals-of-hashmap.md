# Internals of HashMap

{% hint style="info" %}
Default size of hashmap is 16
{% endhint %}

## How is HashMap implemented?

### PUT

When we do a `put` operation, the key is converted to **hashcode**. This helps in picking the index in which the key-value pair should be inserted in hashmap.

The hashcode is modded with size maximum capacity of the hashmap to get the bucket index in which it should be inserted.

When we try to insert another key-value pair, the hashcode is generated for the key. If the hashcode generated is pointing to same index in the HashMap, the new key-value pair will be added as a next node to the existing value in the index. It is like a LinkedList.

If same key is given with new value, then the it will generate the hashcode and gind the bucket index, then check if that key is already present in the bucket. If the key is already present, then it is replaced.

### GET

When the key is given, it calculates the hashcode and uses that hashcode to find the index in hashmap bucket. Once it finds the bucket, it will check along the list and gives the value

### CONTAINS\_KEY

***

## Why Hashmap maximum size is $$1 << 30$$

Thats cause the maximum size is expressed in integer. The range of integer is $$[-2^{31}\: to \: 2^{31} - 1]$$. If the maximum size allowed is $$2^{31}$$, then it could cause integer overflow. So it is restricted to $$2^{30}$$

## How does HashMap allocate size when it is given by the user?

For example if the user has initialized the hashmap as:

```java
Map<Integer, Integer> map = new HashMap<>(1000);
```

Then it won't exactly allocate size of 1000. It will check what is the next highest power of 2 which is just greater than 1000. The next highest power of 2 which is greater than 1000 is $$2^{10}$$, i.e. 1024. So when you ask for 1000 buckets space, it actually allocates you 1024 buckets space.

***

## How is collision handled in HashMap?

This can be answered based on contract between `hashcode` and `equals`.&#x20;

* **Equals Means Same Hash Code:** If two objects are considered equal using the `equals()` method (i.e., `obj1.equals(obj2) == true`), then their hash codes returned by `hashCode()` must also be equal (i.e., `obj1.hashCode() == obj2.hashCode()`).
* **Unequal Objects Can Collide:** Objects that are not equal according to `equals()` can still have the same hash code. This is called a hash collision and while not ideal, it can happen due to hash function limitations.
* **Keep Hash Code Consistent with Equality:** When an object's state changes in a way that affects how it compares for equality with other objects, the object's hash code should ideally be updated to reflect this change. This ensures consistency when using the object in hash-based collections. (Java itself doesn't enforce this, so it's the programmer's responsibility to maintain consistency).\*\*

***

## How hashing happens in HashMap?

```java
/**
     * Computes key.hashCode() and spreads (XORs) higher bits of hash
     * to lower.  Because the table uses power-of-two masking, sets of
     * hashes that vary only in bits above the current mask will
     * always collide. (Among known examples are sets of Float keys
     * holding consecutive whole numbers in small tables.)  So we
     * apply a transform that spreads the impact of higher bits
     * downward. There is a tradeoff between speed, utility, and
     * quality of bit-spreading. Because many common sets of hashes
     * are already reasonably distributed (so don't benefit from
     * spreading), and because we use trees to handle large sets of
     * collisions in bins, we just XOR some shifted bits in the
     * cheapest possible way to reduce systematic lossage, as well as
     * to incorporate impact of the highest bits that would otherwise
     * never be used in index calculations because of table bounds.
     */
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

The above code represents how a hash is calculated for the key.&#x20;

{% hint style="info" %}
If key is null, the hash value is always zero
{% endhint %}

* If the `key` is not null, it calls `key.hashCode()` to retrieve the object's default hash code. This value is stored in the `h` variable.
* The code then performs a bitwise XOR operation between `h` and `h >>> 16`. This operation does the following:
  * `h >>> 16`: Shifts the bits of `h` 16 positions to the right, effectively removing the lower 16 bits and keeping the upper 16 bits. - <mark style="color:yellow;">Gets the 16 MOST RIGHT SIGNIFICANT BITS</mark>
  * `^`: <mark style="background-color:yellow;">XOR operation combines corresponding bits of</mark> <mark style="background-color:yellow;"></mark><mark style="background-color:yellow;">`h`</mark> <mark style="background-color:yellow;"></mark><mark style="background-color:yellow;">and the shifted</mark> <mark style="background-color:yellow;"></mark><mark style="background-color:yellow;">`h`</mark><mark style="background-color:yellow;">, resulting in a value where bits from both the upper and lower parts of the original hash code are mixed.</mark>
  * This technique, known as _hash spreading_, helps to distribute hash codes more evenly, reducing the likelihood of collisions, especially in smaller hash tables.

***

## What is load factor?

Load factor is a threshold value which determines when the hashmap size needs to be increased.

{% hint style="info" %}
The default load factor value is 0.75
{% endhint %}

When the load factor thrshold is reached, it constructs a new map with same mappings with increased capacity generally twice the size of original hashmap.

***

What is TREEIFY\_THRESHOLD?

The bin count threshold for using a tree rather than list for a bin. <mark style="color:yellow;">Bins are converted to trees when adding an element to a bin with at least TREEIFY\_THRESHOLD nodes</mark>. The value must be greater than 2 and should be at least 8 to mesh with assumptions in tree removal about conversion back to plain bins upon shrinkage.

By default, if the bin has 8 elements and a new node should be added, it is converted into a tree bin.

It creates a balanced binary tree, so that the lookup is at almost constant time.&#x20;

## References:

{% embed url="https://www.prepbytes.com/blog/java/internal-working-of-hashmap/" %}

{% embed url="https://levelup.gitconnected.com/internal-working-of-hashmap-in-java-latest-updated-4c2708f76d2c" %}

{% embed url="https://www.youtube.com/watch?v=AsAymWn7D40" %}
