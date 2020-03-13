
### Game of Life

According to the Wikipedia's article: "The __Game of Life__, also known simply as __Life__,</br> 
is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with *m* by *n* cells, each cell has an initial state live (1) or dead (0).</br> 
Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules</br> 
(taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population..
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state.</br> 
The next state is created by applying the above rules simultaneously to every cell in the current state,</br> 
where births and deaths occur simultaneously.

__Example:__
```
Input: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

__Follow up:__
1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

### Solution
__O(n\*m\*8):__
```Swift
class Solution {
    
    // As each cell can only take on value 0 or 1, we can use all 63 bits other
    // than bit 0 to represent the next state, while preserving bit 0 (the original state)
    // for next state computations
    func gameOfLife(_ board: inout [[Int]]) {
        
        // Iterate through each cell of the board
        for row in 0..<board.count {
            for col in 0..<board.first!.count {
                switch (board[row][col], liveNeighbors(board, row, col)) {
                    
                    // Dead cell & exactly 3 live neighbors
                    // Cell will come alive: set bit 1 to 1
                    case (0, 3):
                    board[row][col] |= 1<<1
                    
                    // Live cell with less than 2 or more than 3 live neighbors
                    // Cell will die: do nothing as bit 1 is already 0
                    case (1, 0..<2), (1, 4...8):
                    break
                    
                    // In other cases, preserve the original state
                    // Copy bit 0 over to bit 1
                    default:
                    board[row][col] |= board[row][col]<<1
                }
            }
        }
        
        // Go over the board again & update the new state of each cell
        for row in 0..<board.count {
            for col in 0..<board.first!.count {
                board[row][col] >>= 1
            }
        }
    }
    
    // Helper method to sum the number of live neighbors
    func liveNeighbors(_ board: [[Int]], _ row: Int, _ col: Int) -> Int {
        return [1, -1].reduce(0) {
            $0 + 
            state(board, row-$1, col) + 
            state(board, row, col-$1) + 
            state(board, row-$1, col-$1) + 
            state(board, row-$1, col+$1)
        }
    }
    
    // Helper method to evaluate state of neighboring cells
    // Returned value is either 0 or 1
    func state(_ board: [[Int]], _ row: Int, _ col: Int) -> Int {
        switch (row, col) {
            // We're still within the boundaries of the board
            // Return bit 0 of the cell's value
            case (0..<board.count, 0..<board.first!.count):
            return board[row][col] & 1
            default:
            return 0
        }
    }
}
```