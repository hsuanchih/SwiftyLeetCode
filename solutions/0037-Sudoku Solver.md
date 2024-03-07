
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
    func solveSudoku(_ board: inout [[Character]]) {
        solve(&board, 0, 0)
    }

    func solve(_ board: inout [[Character]], _ row: Int, _ col: Int) -> Bool {
        switch (row, col) {
        case (board.count, _):

            // If we've reached the end of the board,
            // there is a solution to the sudoku
            return true
        case (let row, board.first!.count):

            // If we've reached the end of the current row, 
            // continue to solve the next row
            return solve(&board, row + 1, 0)
        case (let row, let col) where board[row][col] == ".":

            // If the current cell is not filled validate numbers 1...9 to see 
            // if it fits the cell
            for num in 1 ... board.count where isValid(board, row, col, num) {
                board[row][col] = Character(String(num))
                if solve(&board, row, col + 1) {
                    return true
                }
            }
            board[row][col] = "."
            return false
        case (let row, let col):

            // If the current cell is already filled, continue to the next column
            return solve(&board, row, col + 1)
        }
    }

    func isValid(_ board: [[Character]], _ row: Int, _ col: Int, _ num: Int) -> Bool {
        let num: Character = Character(String(num))

        // If num is already used in the corresponding row or col,
        // num cannot be used again
        for i in 0 ..< board.count where board[row][i] == num || board[i][col] == num {
            return false
        }

        // If num is already used in the corresponding 3 by 3 grid
        // num cannot be used again
        for c in col / 3 * 3 ..< col / 3 * 3 + 3 {
            for r in row / 3 * 3 ..< row / 3 * 3 + 3 where board[r][c] == num {
                return false
            }
        }
        return true
    }
}
```
