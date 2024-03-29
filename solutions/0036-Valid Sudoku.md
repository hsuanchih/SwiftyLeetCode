
### Valid Sudoku

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated __according to the following rules__:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the 9 `3x3` sub-boxes of the grid must contain the digits `1-9` without repetition.

![A partially filled sudoku which is valid.](images/question_36.png)

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

__Example 1:__
```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```
__Example 2:__
```
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: 
Same as Example 1, except with the 5 in the top left corner being modified to 8. 
Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```
__Note:__

* A Sudoku board (partially filled) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.
* The given board contain only digits 1-9 and the character '.'.
* The given board size is always 9x9.

### Solution
```Swift
class Solution {
    func isValidSudoku(_ board: [[Character]]) -> Bool {
        var board: [[Character]] = board
        for row in 0 ..< board.count {
            for col in 0 ..< board.first!.count where board[row][col] != "." {
                if !isValid(board, row, col) {
                    return false
                } else {
                    board[row][col] = "."
                }
            }
        }
        return true
    }

    func isValid(_ board: [[Character]], _ row: Int, _ col: Int) -> Bool {
        var seen: Set<Character> = []
        for r in 0 ..< board.count where board[r][col] != "." {
            let element: Character = board[r][col]
            if seen.contains(element) {
                return false
            } else {
                seen.insert(element)
            }
        }

        seen = []
        for c in 0 ..< board.first!.count where board[row][c] != "." {
            let element: Character = board[row][c]
            if seen.contains(element) {
                return false
            } else {
                seen.insert(element)
            }
        }

        seen = []
        for r in row / 3 * 3 ..< row / 3 * 3 + 3 {
            for c in col / 3 * 3 ..< col / 3 * 3 + 3 where board[r][c] != "." {
                let element: Character = board[r][c]
                if seen.contains(element) {
                    return false
                } else {
                    seen.insert(element)
                }
            }
        }
        return true
    }
}
```
