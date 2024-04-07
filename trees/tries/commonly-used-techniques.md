# Commonly Used techniques

## Technique 1

* You can use the general operations provided by Trie and modify the logic according to the question

***

## Technique 2

```
WordDict = [abc, xyz] # Will be transformed to Trie
Stream of chars = 'a','x','y','z'
```

* Sometimes when there is a problem which asks you to deal with stream of characters, then find if any substring is present in the Trie:
* Invert the implementation of Trie, i.e generally you build the Trie by traversing the word from Left -> Right. But build the Trie from Right -> Left
* Since the stream of characters come in, it might have unwanted characters towards your left most
  * For example: in the above code, with the stream of character, the string would be `axyz`
  * The substring `xyz` is present in Trie.
* As we have Trie build from Right -> Left, we do not need to worry about the combinations, about where to break the string and check other possibilities.
* Since we are iterating from Right -> Left, if we find any word as present in the Trie, we can simply give true

***

