# 211. Design Add and Search Words Data Structure (欠)

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the WordDictionary class:

**WordDictionary()** Initializes the object.
void addWord(word) Adds word to the data structure, it can be matched later.
**bool search(word)** Returns true if there is any string in the data structure that matches word or false otherwise. word may contain** dots '.' **where dots can be **matched with any letter**.
 
 ```
 Example:

Input
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
Output
[null,null,null,null,false,true,true,true]

Explanation
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
```

## Solution 1 (DFS)
```java
class WordDictionary {
    class Trie {
        public boolean end;
        public Trie[] children;
        public Trie() {
            end = false;
            children = new Trie[26];
        }
    }
    /** Initialize your data structure here. */
    public Trie trie;
    public WordDictionary() {
        trie = new Trie();
    }
    
    public void addWord(String word) {
        int n = word.length();
        Trie cur = trie;
        for (int i = 0; i < n; ++i) {
            int curIndex = word.charAt(i) - 'a';
            if (cur.children[curIndex] == null) {
                cur.children[curIndex] = new Trie();
            }
            cur = cur.children[curIndex];
        }
        cur.end = true;
    }
    
    public boolean search(String word) {
        int n = word.length();
        return dfs(word, 0, n, trie);
    }

    public boolean dfs(String word, int loc, int n, Trie tr) {
        if (loc == n ) {
            if (tr.end == true) return true;
            return false;
        }
        if (word.charAt(loc) == '.') {
            for (int i = 0; i < 26; ++i) {
                if (tr.children[i] != null) {
                    if (dfs(word, loc + 1, n, tr.children[i])) {
                        return true;
                    }
                }
            }
        } else {
            int curIndex = word.charAt(loc) - 'a';
            if (tr.children[curIndex] != null) {
                if (dfs (word, loc + 1, n, tr.children[curIndex])) {
                    return true;
                }
            }
        }
        return false;
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
 ```