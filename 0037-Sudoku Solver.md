
### Sudoku Solver

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy __all of the following rules__:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.
Empty cells are indicated by the character `'.'`.

![A sudoku puzzle...](images/question_37-0.png)
![...and its solution numbers marked in red.](images/question_37-1.png)

__Note:__

* The given board contain only digits 1-9 and the character '.'.
* You may assume that the given Sudoku puzzle will have a single unique solution.
* The given board size is always 9x9.

### Solution
__Recursive:__
```Swift
class Solution {
    class Solution {
    func solveSudoku(_ board: inout [[Character]]) {
        solve(&board, 0, 0)
    }
    
    func solve(_ board: inout [[Character]], _ row: Int, _ col: Int) -> Bool {
        switch (row, col) {

            // Reached the end of the board
            case (board.count, _):
            return true

            // Reached the end of the current row, solve the next row
            case (_, board.count):
            return solve(&board, row+1, 0)

            // Validate numbers 1...9 if element is '.', otherwise move on to the next column
            default:
            if board[row][col] == "." {
                for i in 1...board.count {
                    let num = Character(String(i))
                    if isValid(board, row, col, num) {
                        board[row][col] = num
                        if solve(&board, row, col+1) {
                            return true
                        }
                        board[row][col] = "."
                    }
                }
            } else {
                return solve(&board, row, col+1)
            }
            return false
        }
    }
    
    func isValid(_ board: [[Character]], _ row: Int, _ col: Int, _ num: Character) -> Bool {

        // Check if there are duplicates in each row & col
        for i in 0..<board.count {
            if board[i][col] == num || board[row][i] == num {
                return false
            }
        }

        // Check 3*3 square for duplicates
        let rStart = row/3*3, cStart = col/3*3
        for r in rStart..<rStart+3 {
            for c in cStart..<cStart+3 {
                if board[r][c] == num {
                    return false
                }
            }
        }
        return true
    }
}
```
