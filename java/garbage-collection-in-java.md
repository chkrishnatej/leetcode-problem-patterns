# Garbage Collection in Java

### Garbage Collector

{% embed url="https://www.youtube.com/watch?t=19s&v=UnaNQgzw4zY" %}

![](https://www.evernote.com/shard/s655/res/5b654be2-f3b5-a8c1-516f-d21c1f4e8e45)

## Reading

* ### [Garbage-First Garbage Collector (oracle.com)](https://docs.oracle.com/en/java/javase/11/gctuning/garbage-first-garbage-collector.html#GUID-ED3AB6D3-FD9B-4447-9EDF-983ED2F7A573)
* [The Parallel Collector (oracle.com)](https://docs.oracle.com/en/java/javase/11/gctuning/parallel-collector1.html#GUID-DCDD6E46-0406-41D1-AB49-FB96A50EB9CE)

## Why garbage collector?

* Relieves programmer the burden of managing memory which makes it easy to develop complex applications with little overhead
* The default GC used in Java8 is Parallel GC
* The default GC used in Java11 is G1GC

## **Objects in garbage collector**

* **Live object** - The object referenced in heap can be traced back to Main thread as at some point the Main thread creates the object graph
* **Dead object** - Unreachable objects which are lying in the heap with no context

### Pre:

* Objects allocated in Java are allocated in "heap" of Java memory
* Static members, class definitions(metadata) are stored in `Metaspace` of the heap
* GC is carried by  daemon thread called `Garbage Collector`
* GC cannot be forced to happen by calling `System.gc()`
* If the heap is full,, it throws `java.lang.OutOfMemoryError` which means there is a memory leak
* The  GC is a <mark style="color:yellow;">**STOP-THE-WORLD**</mark> event, it means all the application threads are stopped until the GC event is completed. Both the minor and major GC are STOP-THE-WORLD events.
* Java GC is called as generational GC.

## Steps involved in GC:

### **Mark**

* Starts from `Main` and walks the object graph, marks the objects that are reachable as LIVE
* ![](https://www.evernote.com/shard/s655/res/fc17f848-3161-0b89-2596-0b4a73e027c7)

### Delete/Sweep

* Deletes the unreachable objects
* ![](https://www.evernote.com/shard/s655/res/21c7dcd3-65a3-31e2-b3e3-5a412af3b0a9)

### Compact

* When deletion happens in heap, it creates some holes in between making it defragmented
* Then compaction happens which makes it as a contagious block of memory
* This is an time consuming step as it has to run through full heap and arrange the memory
* ![](https://www.evernote.com/shard/s655/res/b8e22a52-7836-f68e-362f-aa28cb1f5302)

## Generational collectors

It is called as generational because , the GC is run based on generation of your objects if it is old or young

GC has two spaces

### Young Generation Space

* It if further divided into three spaces\\
* **Eden Space** -`new` objects are created
* **Survivor Space from** -  when Eden space is full, a minor GC is kicked in which deletes all the unused objects and moves the reachable objects to this space and new objects are created in Eden Space. It also marks the objects with number of GC life cycle it endured
* **Survivor Space To** - When a minor GC is run and after the first round, it runs across the Survivor Space from and Eden Space, it marks every object with cycles of GC it has survived, and instead of compacting, move it here from both Survivor Space from and Eden Space. This helps in avoiding compacting step, which is generally pretty heavy operation
* The compaction in Young generation is avoided by moving the objects from one survivor space to another.

{% hint style="info" %}
After a threshold is reached, major GC is kicked in where, the objects which survived the threshold number of GC cycle is promoted to Old generation, the threshold can be set using `-XX MaxTenuringThreshold`\
\
Default value for `-XX MaxTenuringThreshold` in Java8 default GC(Parallel GC) is 15
{% endhint %}



### Old (Tenured) Generation

* Holds the objects for a long time
* Example is cache. When created it is created in Eden space and after surviving through multiple generations of GC, it is pushed to Old generation
* When the old generation is also getting filled up, then major GC gets kicked in which does GC across the entire heap(both young and old)
* Minor GC happens in younger generation and Major GC happens across both Young and old generation
* Both Minor and Major GC are STOP-THE-WORLD events

## Types of GC:

### Serial collector

Mainly for single-cpu machine.Algorithm:It use a single thread to handle heap, and perform stop-the-world pause during any gc. Just see it as toy.This is default for client-class machine _(32bit jvm on windows or single-cpu machine)_.

### Parallel collector

Algorithm:It uses multiple gc threads to handle heap, and perform stop-the-world pause during any gc.<= Java 8, this is default for server-class machine (multi-cpu unix-like machine or any 64bit jvm).

### CMS collector

It's designed to eliminate the long pause associated with the full gc of parallel & serial collector.Algorithm:It use 1 or more gc threads to scan the old generation periodically, and discard unused objects, the pause is very short, but use more cpu time._Warning_: since Java 14, it's removed.

### G1 collector

It's low pause / server style gc, mainly for large heap (> 4Gb).Algorithm:

* Similar as CMS, it use multiple background gc threads to scan & clear heap.
* It divide old generation into parts, it could clean old generation by copy from 1 part to another.\
  Thus it's less possible to get fragmentation.

Since Java 9, this is default for server-class machine (multi-cpu unix-like machine or any 64bit jvm).

### Why use G1 as default?

The main reason is to reduce the gc pause time, though the overall throughput might be reduced.

***

### Objects stored

* [Java Heap Space vs. Stack Memory: How Java Applications Allocate Memory – Stackify](https://stackify.com/java-heap-vs-stack/)

As we talk about heap and stack, there comes a question, where the objects are actually stored. The answer is:

* Actual objects are stored in heap
* The references to objects are stored in stack

***

### JVM Architecture

[https://www.youtube.com/watch?v=ZBJ0u9MaKtM\&t=565s](https://www.youtube.com/watch?v=ZBJ0u9MaKtM\&t=565s)

* [JVM Tutorial - Java Virtual Machine Architecture Explained for Beginners (freecodecamp.org)](https://www.freecodecamp.org/news/jvm-tutorial-java-virtual-machine-architecture-explained-for-beginners/)
* ![](https://www.evernote.com/shard/s655/res/2c5788fd-95aa-c719-da79-300516a851f3)
* <br>

***

### HashCode and equals

* Effective Java book has about it

[https://www.youtube.com/watch?v=IwUwIrz9Ge8](https://www.youtube.com/watch?v=IwUwIrz9Ge8)<br>

***

### HashMaps

\
[https://www.youtube.com/watch?v=c3RVW3KGIIE](https://www.youtube.com/watch?v=c3RVW3KGIIE)[https://www.youtube.com/watch?v=APL28XpFP0c](https://www.youtube.com/watch?v=APL28XpFP0c)<br>

***

### Java Strings

\
[https://www.youtube.com/watch?v=6pLEwJP1Afk](https://www.youtube.com/watch?v=6pLEwJP1Afk)<br>

***

### Java collections

\
Java collections are ADT provided in `util` package. They have most commonly used Data structures.![](https://www.evernote.com/shard/s655/res/f0932c5d-fd59-81f4-51d9-63d416ccf49f)

#### Interfaces:

* Set
*
  * Sorted
  * NavigableSet
* List
* Queue
*
  * Deque
* Map
*
  * SortedMap
  * NavigableMap

\
What is difference between `synchronized map` and `concurrent map` ?

* `synchronized map` puts an object level lock
* `concurrent map`
*
  * No lock at object level
  * locks at hashmap bucket level
  * does not throw `ConcurrentModificationException`

***

### Converting String to Integer

The given string can be converted to Integer or number type using the following ways

* `Integer.parseInt()` - when a valid numerical string is passed it returns a "primitive" integer with data type as `int` , and throws `NumberFormatException` if invalid string is given
* `Integer.valueOf()` - Returns `Integer` object, throws same `NumberFormatException` if invalid string is given

\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
<br>
