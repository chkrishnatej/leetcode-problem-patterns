# 😭 Design Phone Directory

### Problem

[`https://leetcode.com/problems/design-phone-directory`](https://leetcode.com/problems/design-phone-directory)

![](<../../.gitbook/assets/image (24).png>)![](<../../.gitbook/assets/image (21) (1).png>)

### Understanding of problem

At first, I thought I could solve it using a boolean array and a tracker. But not sure if it is a scalable solution as a lot of memory needs to be allocated for it.

The idea is to use a LinkedList so that only required nodes are created and not a burden on the memory. We can use Java LinkedList.



### Proposed solutions

{% code title="BitSetPhoneDirectory.java" %}
```java
public class PhoneDirectory {

    BitSet bitset;
    int max; // max limit allowed
    int smallestFreeIndex; // current smallest index of the free bit

    public PhoneDirectory(int maxNumbers) {
        this.bitset = new BitSet(maxNumbers);
        this.max = maxNumbers;
    }

    public int get() {
        // handle bitset fully allocated
        if(smallestFreeIndex == max) {
            return -1;
        }
        int num = smallestFreeIndex;
        bitset.set(smallestFreeIndex);
        //Only scan for the next free bit, from the previously known smallest free index
        smallestFreeIndex = bitset.nextClearBit(smallestFreeIndex);
        return num;
    }

    public boolean check(int number) {
        return bitset.get(number) == false;
    }

    public void release(int number) {
        //handle release of unallocated ones
        if(bitset.get(number) == false)
            return;
        bitset.clear(number);
        if(number < smallestFreeIndex) {
            smallestFreeIndex = number;
        }
    }
}
```
{% endcode %}

{% code title="LinkedHashSet.java" %}
```java
	Set<Integer> set;
	    /** Initialize your data structure here
	        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
	    public PhoneDirectory(int maxNumbers) {
	        set = new LinkedHashSet<>();
	        
	        for(int i=0; i<maxNumbers; i++){
	            set.add(i);
	        }
	    }
	    
	    /** Provide a number which is not assigned to anyone.
	        @return - Return an available number. Return -1 if none is available. */
	    public int get() {
	    	Iterator iter = set.iterator();
	    	
	        if(!set.isEmpty()){
	        	int val = (int) iter.next();
	        	set.remove(val);
	        	return val;
	        }
	        return -1;
	    }
	    
	    /** Check if a number is available or not. */
	    public boolean check(int number) {
	       return set.contains(number);
	    }
	    
	    /** Recycle or release a number. */
	    public void release(int number) {
	    	set.add(number);
	    }va
```
{% endcode %}

### Solution
