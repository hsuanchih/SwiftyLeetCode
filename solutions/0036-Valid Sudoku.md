
### Valid Sudoku

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9`] without repetition.

Note:
* A Sudoku board (partially filled) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.

__Example 1:__

![question_36.png](../images/question_36.png)
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

__Constraints:__
* `board.length == 9`
* `board[i].length == 9`
* `board[i][j]` is a digit `1-9` or `'.'`.

### Solution
```Swift
class Solution {
    func isValidSudoku(_ board: [[Character]]) -> Bool {
        for row in 0 ..< board.count {
            for col in 0 ..< board.first!.count where board[row][col] != "." {
                if !isValid(board, row: row, col: col) {
                    return false
                }
            }
        }
        return true
    }

    func isValid(_ board: [[Character]], row: Int, col: Int) -> Bool {
        var seen: Set<Character> = []
        for c in 0 ..< board.first!.count where board[row][c] != "." {
            let element: Character = board[row][c]
            if seen.contains(element) {
                return false
            } else {
                seen.insert(element)
            }
        }
        
        seen = []
        for r in 0 ..< board.count where board[r][col] != "." {
            let element: Character = board[r][col]
            if seen.contains(element) {
                return false
            } else {
                seen.insert(element)
            }
        }

        seen = []
        let rowStart: Int = row / 3 * 3
        let colStart: Int = col / 3 * 3
        for r in rowStart ..< rowStart + 3 {
            for c in colStart ..< colStart + 3 where board[r][c] != "." {
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
