
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
__Array:__
```Swift
class Trie {
    private let root: TrieNode

    init() {
        root = TrieNode()
    }
    
    func insert(_ word: String) {
        var node: TrieNode = root
        for char in word {
            if let next = node.next[char.nodeIndex] {
                node = next
            } else {
                let next: TrieNode = TrieNode()
                node.next[char.nodeIndex] = next
                node = next
            }
        }
        node.isWord = true
    }
    
    func search(_ word: String) -> Bool {
        search(word: word)?.isWord == true
    }
    
    func startsWith(_ prefix: String) -> Bool {
        search(word: prefix) != nil
    }

    private func search(word: String) -> TrieNode? {
        var node: TrieNode = root
        for char in word {
            if let next = node.next[char.nodeIndex] {
                node = next
            } else {
                return nil
            }
        }
        return node
    }
}

extension Trie {
    final class TrieNode {
        var isWord: Bool
        var next: [TrieNode?]

        init() {
            isWord = false
            next = Array(repeating: nil, count: 26)
        }
    }
}

extension Character {
    var nodeIndex: Int {
        Int(asciiValue! - Character("a").asciiValue!)
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