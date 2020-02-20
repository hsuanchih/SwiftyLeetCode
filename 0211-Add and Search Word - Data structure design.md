
### Add and Search Word - Data structure design

Design a data structure that supports the following two operations:
```
void addWord(word)
bool search(word)
```
search(word) can search a literal word or a regular expression string containing only letters `a-z` or `.`.</br> 
A `.` means it can represent any one letter.

__Example:__
```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```
__Note:__
You may assume that all words are consist of lowercase letters `a-z`.

### Solution
```Swift
private extension Character {
    var relativeOffset : Int {
        return Int(asciiValue! - Character("a").asciiValue!)
    }
    
    init(offset: UInt8) {
        self.init(UnicodeScalar(Character("a").asciiValue!+offset))
    }
}

class WordDictionary {
    
    private class TrieNode {
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
    
    /** Adds a word into the data structure. */
    func addWord(_ word: String) {
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
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    func search(_ word: String) -> Bool {
        return search(Array(word), 0, root)
    }
    
    private func search(_ word: [Character], _ index: Int, _ curr: TrieNode) -> Bool {
        if index == word.count {
            if curr.isWord {
                return true
            }
            return false
        }
        switch word[index] {
        case ".":
            for node in ((UInt8(0)..<26).compactMap { curr.node(for: Character(offset: $0)) }) {
                if search(word, index+1, node) {
                    return true
                }
            }
        case let char:
            if let node = curr.node(for: char) {
                return search(word, index+1, node)
            }
        }
        return false
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * let obj = WordDictionary()
 * obj.addWord(word)
 * let ret_2: Bool = obj.search(word)
 */
```