# Tries

## What is a Trie?

A Trie (pronounced "try") is a special data structure that helps store and search for words efficiently

***

## **Why Use a Trie?**

* **Fast Word Search:** Instead of searching through an entire list, a Trie lets you find words based on their starting letters. Like following branches in a tree, you can quickly eliminate parts of the alphabet that don't match your search.
* **Prefix Power:** Say you're typing a word on your phone. A Trie can suggest words based on the letters you've typed so far, making it super helpful for autocorrect and autocomplete features.

### **Benefits:**

* **Speedy Searches:** Finding words with a Trie is like taking shortcuts in a maze. You only explore relevant parts, making searches faster!
* **Prefix Magic:** Need words that start with a specific combination of letters? A Trie can find them in a flash!

***

## **Where are Tries used?**

* **Spell Checkers:** Tries help identify misspelled words by suggesting corrections based on valid prefixes.
* **Search Engines:** Tries can power autocomplete features, suggesting relevant searches as you type.
* **Social Media:** When you tag someone on social media, Tries might be behind the scenes, helping you find the right person quickly.

***

## Time and Space complexity

| Operation | Time Complexity | Space Complexity | Notes                                                                   |
| --------- | --------------- | ---------------- | ----------------------------------------------------------------------- |
| Search    | O(k)            | O(W\*L)          | k - length of search word, W - number of words, L - average word length |
| Insertion | O(k)            | O(W\*L)          | Similar to search                                                       |
| Deletion  | O(k)            | O(W\*L)          | In worst case (deleting entire branch)                                  |

***

## Code

```java
// TrieNode class
class TrieNode {
    // An array of TrieNode objects, where each index represents a character in the alphabet.
    public TrieNode[] next;
    // Whether the current node represents the end of a word.
    public boolean isWord;

    public TrieNode() {
        this.next = new TrieNode[26];
        this.isWord = false;
    }
}

// Trie class
class Trie {

    // The root node of the trie.
    public TrieNode root;

    // Constructor.
    public Trie() {
        root = = new TrieNode();
    }

    // Inserts the given word into the trie.
    public void insert(String word) {
        // Start at the root node.
        TrieNode node = root;

        // Iterate over the characters in the word.
        for (char ch: word.toCharArray()) {
            // Get the index of the current character in the alphabet.
            int i = ch - 'a';

            // If there is no TrieNode object at the current index, create one.
            if (node.next[i] == null) {
                node.next[i] = new TrieNode();
            }

            // Move to the next TrieNode object.
            node = node.next[i];
        }

        // Set the isWord flag to true for the leaf node.
        node.isWord = true;
    }

    // Finds the TrieNode object for the given word in the trie.
    public TrieNode find(String word) {
        // Start at the root node.
        TrieNode node = root;

        // Iterate over the characters in the word.
        for (char ch: word.toCharArray()) {
            // Get the index of the current character in the alphabet.
            int i = ch - 'a';

            // If there is no TrieNode object at the current index, return null.
            if (node.next[i] == null) {
                return null;
            }

            // Move to the next TrieNode object.
            node = node.next[i];
        }

        // Return the TrieNode object for the given word.
        return node;
    }

    // Searches for the given word in the trie.
    public boolean search(String word) {
        // Find the TrieNode object for the given word in the trie.
        TrieNode node = find(word);

        // Return true if the TrieNode object is not null and the isWord flag is true, false otherwise.
        return node != null && node.isWord;
    }

    // Checks if the given prefix starts any word in the trie.
    public boolean startsWith(String prefix) {
        // Find the TrieNode object for the given prefix in the trie.
        TrieNode node = find(prefix);

        // Return true if the TrieNode object is not null, false otherwise.
        return node != null;
    }
}


/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```
