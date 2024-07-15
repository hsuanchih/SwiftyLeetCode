
### Design Add and Search Words Data Structure

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:
* `WordDictionary()` Initializes the object.
* `void addWord(word)` Adds `word` to the data structure, it can be matched later.
* `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.
 

__Example:__
```
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
 
__Constraints:__
* `1 <= word.length <= 25`
* `word` in `addWord` consists of lowercase English letters.
* `word` in `search` consist of `'.'` or lowercase English letters.
* There will be at most `2` dots in `word` for `search` queries.
* At most `pow(10, 4)` calls will be made to `addWord` and `search`.

### Solution
__C(word, 1) + C(word, 2) Time Add, Constant Time Search, TLE:__
```Swift
class WordDictionary {
    private var words: Set<String>

    init() {
        words = []
    }
    
    // C(word, 1) + C(word, 2) Time
    func addWord(_ word: String) {
        guard !words.contains(word) else { return }
        var temp: [Character] = []
        generateMatches(word, &temp, 2, &words)
    }
    
    // Constant Time
    func search(_ word: String) -> Bool {
        words.contains(word)
    }

    private func generateMatches(_ word: String, _ temp: inout [Character], _ wildcards: Int, _ words: inout Set<String>) {
        if temp.count == word.count {
            words.insert(String(temp))
        } else {
            let index = temp.count
            for i in word.indices[word.index(word.startIndex, offsetBy: index) ..< word.endIndex] {
                temp.append(word[i])
                generateMatches(word, &temp, wildcards, &words)
                temp.removeLast()
                if wildcards > 0 {
                    temp.append(".")
                    generateMatches(word, &temp, wildcards - 1, &words)
                    temp.removeLast()
                }
            }
        }
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * let obj = WordDictionary()
 * obj.addWord(word)
 * let ret_2: Bool = obj.search(word)
 */
```
__O(word) Time Add, O(26 * word) Time Search:__
```Swift
class WordDictionary {
    private let root: TrieNode

    init() {
        root = TrieNode()
    }
    
    func addWord(_ word: String) {
        var node: TrieNode = root
        for char in word {
            if let next = node.lookup[char] {
                node = next
            } else {
                let next = TrieNode()
                node.lookup[char] = next
                node = next
            }
        }
        node.isWord = true
    }
    
    func search(_ word: String) -> Bool {
        search(word, word.startIndex, root)
    }

    private func search(_ word: String, _ index: String.Index, _ node: TrieNode) -> Bool {
        if index == word.endIndex {
            return node.isWord
        } else {
            let char: Character = word[index]
            if char == "." {
                for next in node.lookup.values where search(word, word.index(index, offsetBy: 1), next) {
                    return true
                }
            } else if let next = node.lookup[char] {
                return search(word, word.index(index, offsetBy: 1), next)
            }
            return false
        }
    }
}

extension WordDictionary {
    final class TrieNode {
        var isWord: Bool
        var lookup: [Character: TrieNode]

        init() {
            isWord = false
            lookup = [:]
        }
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * let obj = WordDictionary()
 * obj.addWord(word)
 * let ret_2: Bool = obj.search(word)
 */
```
__O(word) Time Add, O(26 * word) Time Search:__
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