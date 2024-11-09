
### Word Search II

Given an `m x n` `board` of characters and a list of strings `words`, return __all words on the board__.

Each word must be constructed from letters of sequentially __adjacent cells__, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

__Example:__
```
Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```

__Note:__
1. All inputs are consist of lowercase letters a-z.
2. The values of `words` are distinct.

### Solution
__O((row\*col)^2+k\*words) Time, O(k\*words) Space - Prefix Tree + DFS Traversal:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue!-Character("a").asciiValue!)
    }
}

class Solution {
    
    // TrieNode data structure declaration
    private class TrieNode {
        var nodes : [TrieNode?] = Array(repeating: nil, count: 26)
        var word: String?
    }
    // Helper method to add words to the Trie
    private func add(_ word: String) {
        var curr = root
        for char in word {
            let offset = char.offset, node = curr.nodes[offset] ?? TrieNode()
            curr.nodes[offset] = node
            curr = node
        }
        curr.word = word
    }
    
    // Root node of the Trie
    private var root = TrieNode()
    
    func findWords(_ board: [[Character]], _ words: [String]) -> [String] {
        var board = board, result : Set<String> = []
        words.forEach(add)
        for row in 0..<board.count {
            for col in 0..<(board.first?.count ?? 0) {
                walk(&board, row, col, root, &result)
            }
        }
        return Array(result)
    }
    
    // Helper method to walk the board & record words seen
    private func walk(_ board: inout [[Character]], _ row: Int, _ col: Int, _ curr: TrieNode, _ result: inout Set<String>) {
        switch (row, col) {
            case (0..<board.count, 0..<(board.first?.count ?? 0)) where board[row][col] != "%":
            let char = board[row][col]
            if let node = curr.nodes[char.offset] {
                if let word = node.word {
                    result.insert(word)
                }
                board[row][col] = "%"
                for next in [-1, 1] {
                    walk(&board, row+next, col, node, &result)
                    walk(&board, row, col+next, node, &result)
                }
                board[row][col] = char
            }
            default:
            break
        }
    }
}
```