
### Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

__Example:__
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

### Solution
__O(m\*n\*4^L):__
```Swift
class Solution {
    func exist(_ board: [[Character]], _ word: String) -> Bool {
        if word.isEmpty {
            return true
        }
        var board = board, word = Array(word)
        for row in stride(from: 0, to: board.count, by: 1) {
            for col in stride(from: 0, to: board.first!.count, by: 1) {
                if isValid(&board, word, 0, row, col) {
                    return true
                }
            }
        }
        return false
    }
    
    func isValid(_ board: inout [[Character]], _ word: [Character], _ index: Int, _ row: Int, _ col: Int) -> Bool {
        if index == word.count {
            return true
        }
        switch (row, col) {
            case (0..<board.count, 0..<board.first!.count):
            if board[row][col] == word[index] {
                let curr = board[row][col]
                board[row][col] = "*"
                if isValid(&board, word, index+1, row+1, col) || 
                isValid(&board, word, index+1, row-1, col) ||
                isValid(&board, word, index+1, row, col+1) ||
                isValid(&board, word, index+1, row, col-1) {
                    return true
                }
                board[row][col] = curr
            }
            fallthrough
            default:
            return false
        }
    }
}
```