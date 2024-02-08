
### Surrounded Regions

Given an `m x n` matrix board containing `'X'` and `'O'`, capture all regions that are 4-directionally surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

__Example 1:__

![example1](../images/question_130.jpg)
```
Input: board = [
    ["X","X","X","X"],
    ["X","O","O","X"],
    ["X","X","O","X"],
    ["X","O","X","X"]
]
Output: [
    ["X","X","X","X"],
    ["X","X","X","X"],
    ["X","X","X","X"],
    ["X","O","X","X"]
]
Explanation: Notice that an 'O' should not be flipped if:
- It is on the border, or
- It is adjacent to an 'O' that should not be flipped.
The bottom 'O' is on the border, so it is not flipped.
The other three 'O' form a surrounded region, so they are flipped.
```
__Example 2:__

```
Input: board = [["X"]]
Output: [["X"]]
```

__Constraints:__
* `m == board.length`
* `n == board[i].length`
* `1 <= m, n <= 200`
* `board[i][j]` is `'X'` or `'O'`.

### Solution
```Swift
class Solution {
    func solve(_ board: inout [[Character]]) {
        for row in 0 ..< board.count {
            for col in [0, board.first!.count - 1] where board[row][col] == "O" {
                markBorderConnections(&board, row: row, col: col)
            }
        }
        for col in 0 ..< board.first!.count {
            for row in [0, board.count - 1] where board[row][col] == "O" {
                markBorderConnections(&board, row: row, col: col)
            }
        }
        for row in 0 ..< board.count {
            for col in 0 ..< board.first!.count {
                switch board[row][col] {
                case "B":
                    board[row][col] = "O"
                case "O":
                    board[row][col] = "X"
                default:
                    break
                }
            }
        }
    }

    func markBorderConnections(_ board: inout [[Character]], row: Int, col: Int) {
        switch (row, col) {
        case (0 ..< board.count, 0 ..< board.first!.count) where board[row][col] == "O":
            board[row][col] = "B"
            [-1, 1].forEach {
                markBorderConnections(&board, row: row + $0, col: col)
                markBorderConnections(&board, row: row, col: col + $0)
            }
        default:
            break
        }
    }
}
```
