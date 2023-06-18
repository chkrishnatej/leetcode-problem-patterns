# Scheduling

has come up with the [HashMap/TreeMap algorithm](https://leetcode.com/problems/my-calendar-iii/discuss/109556/JavaC++-Clean-Code) to sort the intervals and record the overlaps. The algorithm can be applied to the aforementioned problems with `O(nlogn)` time complexity.

Here is the idea -

1. Load all intervals to the `TreeMap`, where keys are intervals' start/end boundaries, and values accumulate the changes at that point in time.
2. Traverse the TreeMap (in other words, sweep the timeline). If a new interval starts, increase the counter (`k` value) by 1, and the counter decreases by 1, if an interval has finished.
3. Calcalulate the number of the active ongoing intervals.

In the following graph, there are 4 intervals/meetings, the `k` value is the number of rooms or booking sessions.\


<figure><img src="https://assets.leetcode.com/users/zzhai/image_1544421563.png" alt=""><figcaption></figcaption></figure>

## Problems

* [Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)
* [Insert Interval](https://leetcode.com/problems/insert-interval/description/)
* [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/description/)
* [Meeting Rooms II](http://leetcode.com/problems/meeting-rooms-ii/)
* [My Calendar I](https://leetcode.com/problems/my-calendar-i/description/) (Not the most optimal sol using this approach)
* [My Calendar II](https://leetcode.com/problems/my-calendar-ii/description/) (Not the most optimal sol using this approach)
* [My Calendar III](https://leetcode.com/problems/my-calendar-iii/description/) (Not the most optimal sol using this approach)
  * [https://leetcode.com/problems/my-calendar-iii/solutions/109556/JavaC++-Clean-Code/](https://leetcode.com/problems/my-calendar-iii/solutions/109556/JavaC++-Clean-Code/)
* [Employee Free Time](https://leetcode.com/problems/employee-free-time/discuss/175081/Sweep-line-Java-with-Explanations)
* [Car Pooling](https://leetcode.com/problems/car-pooling/)
  * > thanks for this, such O(n(log(n)) approach using sorted map (TreeMap/RedBlack Tree) also works for car pooling ([https://leetcode.com/problems/car-pooling/](https://leetcode.com/problems/car-pooling/)) but there is a more efficient O(n) solution if number of locations is constrained in range (e.g. from 0 to 1000) : [https://leetcode.com/problems/car-pooling/discuss/488622/Java-with-TreeMap-nlog(n)-very-simple](https://leetcode.com/problems/car-pooling/discuss/488622/Java-with-TreeMap-nlog\(n\)-very-simple)

## Examples

\
**252. Meeting Rooms**

```java
    public boolean canAttendMeetings(Interval[] intervals) {
        Map<Integer, Integer> map = new TreeMap<>();
        for (Interval itl : intervals) {
            map.put(itl.start, map.getOrDefault(itl.start, 0) + 1);
            map.put(itl.end, map.getOrDefault(itl.end, 0) - 1);
        }
        int room = 0; 
        for (int v : map.values()) {
            room += v; 
            if (room > 1) return false; 
        }
        return true; 
    }
```

**253. Meeting Rooms II**

```java
    public int minMeetingRooms(Interval[] intervals) {
        Map<Integer, Integer> map = new TreeMap<>();
        for (Interval itl : intervals) {
            map.put(itl.start, map.getOrDefault(itl.start, 0) + 1);
            map.put(itl.end, map.getOrDefault(itl.end, 0) - 1);
        }
        int room = 0, k = 0; 
        for (int v : map.values()) 
            k = Math.max(k, room += v); 
        
        return k; 
    }
```

**731. My Calendar II**

```java
    private TreeMap<Integer, Integer> map = new TreeMap<>();
    public boolean book(int s, int e) {
        map.put(s, map.getOrDefault(s, 0) + 1); 
        map.put(e, map.getOrDefault(e, 0) - 1); 
		
        int cnt = 0, k = 0;
        for (int v : map.values()) { 
            k = Math.max(k, cnt += v);
            if (k > 2) { 
                map.put(s, map.get(s) - 1); 
                map.put(e, map.get(e) + 1); 
                return false; 
            }
        }
        return true;
    }
```

**732. My Calendar III**

```java
    Map<Integer, Integer> map = new TreeMap<>();
    public int book(int start, int end) {
        map.put(start, map.getOrDefault(start, 0) + 1);
        map.put(end, map.getOrDefault(end, 0) - 1);
        
        int cnt = 0, k = 0;  
        for (int v : map.values()) {
            cnt += v;
            k = Math.max(k, cnt);
        }
        return k; 
    }
```

A bit more complicated:\
**56. Merge Interval**

```java
     public List<Interval> merge(List<Interval> intervals) {
        Map<Integer, Integer> map = new TreeMap<>();
        for (Interval itl : intervals) {
            map.put(itl.start, map.getOrDefault(itl.start, 0) + 1);
            map.put(itl.end, map.getOrDefault(itl.end, 0) - 1);
        }
        List<Interval> list = new LinkedList<>(); 
        int start = 0, cnt = 0; 
        for (Map.Entry<Integer, Integer> e : map.entrySet()) {
            if (cnt == 0) start = e.getKey();
			// if cnt is 0, that means a full interval has been completed. 
            if ((cnt += e.getValue()) == 0) 
				list.add(new Interval(start, e.getKey()));
        }
        return list;
    }
```
