# Comparator in Java

Comparator interfaces helps us in comparing two objects.&#x20;

We can use this comparator interface in data structure like `PriorityQueue` to make min-heap, max-heap or use any custom comparing logic to build the heap.

```java
// Using the comparator Integer.compare(a, b), we can build min heap, i.e
// we can access the minimum element always on top of the queue
PriorityQueue<Integer> minPq = new PriorityQueue<>((a,b) -> Integer.compare(a, b));

// Using the comparator Integer.compare(a, b), we can build max heap, i.e.
// we can access the maximum element always on top of the queue
PriorityQueue<Integer> maxPq = new PriorityQueue<>((a,b) -> Integer.compare(b, a));
```

This is one of the most simpler use case of comparator. We can use custom classes, Maps, Pairs or anything.

\
The Comparator interface is used to compare two objects. It is a functional interface, which means that it can be used as the assignment target for a lambda expression or method reference.

The Comparator interface has a single method, `compare(T o1, T o2)`. This method returns an integer value indicating the relationship between the two objects. The possible return values are:



| Return value | Comparison | Description                                       |
| ------------ | ---------- | ------------------------------------------------- |
| -1           | o1 < o2    | The first value is less than the second value.    |
| 0            | o1 == o2   | The two values are equal.                         |
| 1            | o1 > o2    | The first value is greater than the second value. |
| NULL         |            | One or both of the values is NULL.                |

## Building Custom comparatoe

Lets say we have a class called `DistanceMonitorPair`

```java
class DistanceMonitorPair {
    private int distance;
    private int monitors;

    public DistanceMonitorPair(int distance, int monitors) {
        this.distance = distance;
        this.monitors = monitors;
    }

    public int getDistance() {
        return distance;
    }

    public int getMonitors() {
        return monitors;
    }
}
```

When we want to compare `List<DistanceMonitorPair>` and sort them, we can sort them based on the attributes available.&#x20;

In that case we can write a comparator:

```java
Comparator<DistanceMonitorPair> minDistanceComparator = 
                Comparator.comparing(DistanceMonitorPair::getDistance)
                .thenComparing(DistanceMonitorPair::getMonitors);
```

* By default comparator makes ascending `0...highest` .&#x20;
* And it ther is a tie, then we can use another property in the class to break the tie.
* It follows natural order of sorting. What that means is
  * If it is numbers, it will do ascending i.e. `0..highest`
  * If it is letters, then it will be `a...z`

### Write descending comparator

```java
Comparator<DistanceMonitorPair> maxDistanceComparator =
                Comparator.comparing(DistanceMonitorPair::getDistance).reversed()
                .thenComparing(DistanceMonitorPair::getMonitors);
```

If we want a descending comparator, we can simply use `.reversed()`.  Then, the it will be sorted from `highest... 0`.

### Using the comparator

```java
// We can build min heap, i.e
// we can access the minimum element always on top of the queue
PriorityQueue<DistanceMonitorPair> minPq = new PriorityQueue<>(minDistanceComparator);

 // We can build max heap, i.e.
// We can access the maximum element always on top of the queue
PriorityQueue<DistanceMonitorPair> maxPq = new PriorityQueue<>(maxDistanceComparator);
```
