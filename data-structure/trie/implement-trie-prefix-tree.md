---
description: LeetCode 208
---

# Implement Trie \(Prefix Tree\)

Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**

```text
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

**Note:**

* You may assume that all inputs are consist of lowercase letters `a-z`.
* All inputs are guaranteed to be non-empty strings.

```java
public class Trie {
    
    private Node root;
    private static int R = 26;
    public class Node{
        boolean isEnd;
        Node[] next = new Node[R];
    }
    
    public Trie() {
        // do intialization if necessary
        root = new Node();
    }

    /*
     * @param word: a word
     * @return: nothing
     */
    public void insert(String word) {
        // write your code here
        Node node = root;
        for(int i = 0; i < word.length(); i++){
            char c = word.charAt(i);
            if(node.next[c - 'a'] == null){
                node.next[c - 'a'] = new Node();
            }
            node = node.next[c - 'a'];
        }
        node.isEnd = true;
    }
    
    public Node searchPrefix(String word){
        Node node = root;
        for(int i = 0; i < word.length(); i++){
            char c = word.charAt(i);
            if(node.next[c - 'a'] == null)
                return null;
            else 
                node = node.next[c - 'a'];
        }
        return node;
    }

    /*
     * @param word: A string
     * @return: if the word is in the trie.
     */
    public boolean search(String word) {
        // write your code here
        Node node = searchPrefix(word);
        return node != null && node.isEnd;
    }

    /*
     * @param prefix: A string
     * @return: if there is any word in the trie that starts with the given prefix.
     */
    public boolean startsWith(String prefix) {
        // write your code here
        Node node = searchPrefix(prefix);
        return node != null;
    }
}
```



