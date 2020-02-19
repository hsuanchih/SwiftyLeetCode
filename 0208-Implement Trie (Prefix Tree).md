
### Implement Trie (Prefix Tree)

Implement a trie with `insert`, `search`, and `startsWith` methods.

__Example:__
```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");
trie.search("app");     // returns true
```
__Note:__
* You may assume that all inputs are consist of lowercase letters a-z.
* All inputs are guaranteed to be non-empty strings.

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