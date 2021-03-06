
### Longest Word in Dictionary

Given a list of strings `words` representing an English Dictionary,</br> 
find the longest word in `words` that can be built one character at a time by other words in `words`.</br> 
If there is more than one possible answer, return the longest word with the smallest lexicographical order.

If there is no answer, return the empty string.

__Example 1:__
```
Input: 
words = ["w","wo","wor","worl", "world"]
Output: "world"
Explanation: 
The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".
```
__Example 2:__
```
Input: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
Output: "apple"
Explanation: 
Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".
```

__Note:__
* All the strings in the input will only contain lowercase letters.
* The length of `words` will be in the range `[1, 1000]`.
* The length of `words[i]` will be in the range `[1, 30]`.

### Solution
__O((words\*l)^2+words\*l) Time, O(words\*l) Space - HashSet:__
```Swift
class Solution {
    func longestWord(_ words: [String]) -> String {
        
        var result = ""

        // Put words in a set for constant time lookup
        let lookup = Set(words.map(ArraySlice.init))

        // Iterate through words
        for word in words {

            // If length of current word is less than the length of result, this word will not be our solution
            // If length of current word is the same length as our solution, only consider the word if it's lexicographically
            // larger than our current solution
            if word.count < result.count || (word.count == result.count && word > result) {
                continue
            }

            let curr = Array(word)
            // Construct every prefix of each word
            for i in 0...word.count {

                // If prefix is not in the lookup, the word cannot be constructed from other words in the input
                // Move on to the next word
                let prefix = curr[0..<i]
                if i > 0 && !lookup.contains(prefix) {
                    break
                }

                // If we've reached the last character of the word, the word can be constructed from all other words
                // in the input, update result
                if i == word.count {
                    result = word
                }
            }
        }
        return result
    }
}
```
__O(words\*l+words\*l) Time, O(words\*l) Time - Trie + BFS:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue!-Character("a").asciiValue!)
    }
}
struct Trie {
    private class TrieNode {
        var nodes : [TrieNode?] = Array(repeating: nil, count: 26)
        var word : String?
    }
    private let root = TrieNode()
    mutating public func insert(_ word: String) {
        var curr = root
        for char in word {
            let node = curr.nodes[char.offset] ?? TrieNode()
            if curr.nodes[char.offset] == nil {
                curr.nodes[char.offset] = node
            }
            curr = node
        }
        curr.word = word
    }
    
    public func findLongestWord() -> String {
        var stack : [TrieNode] = [root], result : String = ""
        while !stack.isEmpty {
            let curr = stack.removeLast()
            if let word = curr.word, word.count >= result.count {
                result = word
            }
            curr.nodes.forEach {
                if let node = $0, let _ = node.word {
                    stack.append(node)
                }
            }
        }
        return result
    }
}

class Solution {
    func longestWord(_ words: [String]) -> String {
        var trie = Trie()
        for word in words {
            trie.insert(word)
        }
        return trie.findLongestWord()
    }
}
```