
### Implement Trie (Prefix Tree)

A __trie__ (pronounced as "try") or __prefix tree__ is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:
* `Trie()` Initializes the trie object.
* `void insert(String word)` Inserts the string `word` into the trie.
* `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
* `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.
 
__Example 1:__
```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

__Constraints:__
* `1 <= word.length, prefix.length <= 2000`
* `word` and `prefix` consist only of lowercase English letters.
* At most `3 * pow(10, 4)` calls in total will be made to `insert`, `search`, and `startsWith`.

### Solution
```Swift
private extension Character {
    var relativeOffset : Int {
        return Int(asciiValue! - Character("a").asciiValue!)
    }
}

class Trie {
    class TrieNode {
        var isWord : Bool = false
        private var nodes : [TrieNode?] = Array(repeating: nil, count: 26)
        
        public func node(for character: Character) -> TrieNode? {
            return nodes[character.relativeOffset]
        }
        public func insert(_ character: Character) -> TrieNode {
            let node = TrieNode()
            nodes[character.relativeOffset] = node
            return node
        }
    }
    
    private var root : TrieNode

    /** Initialize your data structure here. */
    init() {
        root = TrieNode()
    }
    
    /** Inserts a word into the trie. */
    func insert(_ word: String) {
        var curr = root
        for char in word {
            if let node = curr.node(for: char) {
                curr = node
            } else {
                curr = curr.insert(char)
            }
        }
        curr.isWord = true
    }
    
    /** Returns if the word is in the trie. */
    func search(_ word: String) -> Bool {
        return searchNode(word)?.isWord ?? false
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    func startsWith(_ prefix: String) -> Bool {
        return searchNode(prefix) != nil
    }
    
    private func searchNode(_ word: String) -> TrieNode? {
        var curr = root
        for char in word {
            guard let node = curr.node(for: char) else { return nil }
            curr = node
        }
        return curr
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * let obj = Trie()
 * obj.insert(word)
 * let ret_2: Bool = obj.search(word)
 * let ret_3: Bool = obj.startsWith(prefix)
 */
```