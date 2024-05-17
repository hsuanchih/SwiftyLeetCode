
### Game of Life

According to Wikipedia's article: "The __Game of Life__, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an `m x n` grid of cells, where each cell has an initial state: __live__ (represented by a `1`) or __dead__ (represented by a `0`). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population.
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously. Given the current state of the `m x n` grid board, return the next state.

__Example 1:__

![question_289-0.jpg](../images/question_289-0.jpg)
```
Input: board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
Output: [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
```
__Example 2:__

![question_289-1.jpg](../images/question_289-1.jpg)
```
Input: board = [[1,1],[1,0]]
Output: [[1,1],[1,1]]
```

__Constraints:__
* `m == board.length`
* `n == board[i].length`
* `1 <= m, n <= 25`
* `board[i][j]` is `0` or `1`.

__Follow up:__
* Could you solve it in-place? Remember that the board needs to be updated simultaneously: You cannot update some cells first and then use their updated values to update other cells.
* In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches upon the border of the array (i.e., live cells reach the border). How would you address these problems?

### Solution
__O(n * m * 8):__
```Swift
class Solution {
    func gameOfLife(_ board: inout [[Int]]) {
        // Use the following values to represent the before & after states:
        // 00 - dead -> dead
        // 01 - live -> dead
        // 10 - dead -> live
        // 11 - live -> live

        for row in 0 ..< board.count {
            for col in 0 ..< board.first!.count {
                switch (board[row][col] & 1, liveNeighbors(board, row: row, col: col)) {
                case (0, 3):
                    // A dead cell turns live if it has exactly 3 live neighbors
                    board[row][col] = 2
                case (1, 2 ... 3):
                    // A live cell remains live only if it has 2 ... 3 live neighbors
                    board[row][col] = 3
                default:
                    break
                }
            }
        }

        for row in 0 ..< board.count {
            for col in 0 ..< board.first!.count {
                // Move to the after state
                board[row][col] = board[row][col] >> 1
            }
        }
    }

    func liveNeighbors(_ board: [[Int]], row: Int, col: Int) -> Int {
        var result: Int = 0
        for rowOffset in -1 ... 1 {
            for colOffset in -1 ... 1 where !(rowOffset == 0 && colOffset == 0) {
                if isLive(board, row: row + rowOffset, col: col + colOffset) {
                    result += 1
                }
            }
        }
        return result
    }

    func isLive(_ board: [[Int]], row: Int, col: Int) -> Bool {
        switch (row, col) {
        case (0 ..< board.count, 0 ..< board.first!.count):
            return board[row][col] & 1 == 1
        default:
            return false
        }
    }
}
```