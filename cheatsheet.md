# CheatSheet

## Which DataStructure to use

### Frequency Calculation

* When you need to do frequency calculation for characters

```java
String s = "abcd"
int[] freqMap = new int[26];
for(char ch: s.toCharArray()){
    freqMap[ch-'a']++;
}
```

* In other cases

```java
Map<Integer, Integer> freqMap = new HashMap<>();
for-loop {
    freqMap.put(num, freqMap.getOrDefault(num, 0)+1);
}
```

> Time Complexity:
>
> `O(n)`
>
> n -> Number of elements in the source

### Quick Lookups

* Use HashSet, it gives better performance when size of the List is bigger and lookups would be fast
* If `List<?>` is given

```java
List<String> strList = Arrays.asList("apple", "bat", "cat");
Set<String> strSet = new HashSet<>(strList);
```

> Time Complexity:
>
> `O(n)`
>
> n -> Number of elements in the source



### Using a custom Comparator

* To sort a list/collection you can use comparators, by default they provide natural orders like ascending and descending
