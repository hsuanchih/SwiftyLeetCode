
### Available Captures for Rook

On an 8 x 8 chessboard, there is one white rook.  There also may be empty squares, white bishops, and black pawns.  These are given as characters 'R', '.', 'B', and 'p' respectively. Uppercase characters represent white pieces, and lowercase characters represent black pieces.

The rook moves as in the rules of Chess: it chooses one of four cardinal directions (north, east, west, and south), then moves in that direction until it chooses to stop, reaches the edge of the board, or captures an opposite colored pawn by moving to the same square it occupies.  Also, rooks cannot move into the same square as other friendly bishops.

Return the number of pawns the rook can capture in one move.

__Example 1:__

![Example 1](images/question_999-0.png)
```
Input: [
[".",".",".",".",".",".",".","."],
[".",".",".","p",".",".",".","."],
[".",".",".","R",".",".",".","p"],
[".",".",".",".",".",".",".","."],
[".",".",".",".",".",".",".","."],
[".",".",".","p",".",".",".","."],
[".",".",".",".",".",".",".","."],
[".",".",".",".",".",".",".","."]
]
Output: 3
Explanation: 
In this example the rook is able to capture all the pawns.
```
__Example 2:__

![Example 2](images/question_999-1.png)
```
Input: [
[".",".",".",".",".",".",".","."],
[".","p","p","p","p","p",".","."],
[".","p","p","B","p","p",".","."],
[".","p","B","R","B","p",".","."],
[".","p","p","B","p","p",".","."],
[".","p","p","p","p","p",".","."],
[".",".",".",".",".",".",".","."],
[".",".",".",".",".",".",".","."]
]
Output: 0
Explanation: 
Bishops are blocking the rook to capture any pawn.
```
__Example 3:__

![Example 3](images/question_999-2.png)
```
Input: [
[".",".",".",".",".",".",".","."],
[".",".",".","p",".",".",".","."],
[".",".",".","p",".",".",".","."],
["p","p",".","R",".","p","B","."],
[".",".",".",".",".",".",".","."],
[".",".",".","B",".",".",".","."],
[".",".",".","p",".",".",".","."],
[".",".",".",".",".",".",".","."]
]
Output: 3
Explanation: 
The rook can capture the pawns at positions b5, d6 and f5.
```

__Note:__
1. `board.length == board[i].length == 8`
2. `board[i][j]` is either `'R'`, `'.'`, `'B'`, or `'p'`
3. There is exactly one cell with `board[i][j] == 'R'`

### Solution
__O(n^2+2*n) Recursive:__
```Swift
class Solution {
    enum CaptureType : CaseIterable {
        case row, col
    }
    
    func numRookCaptures(_ board: [[Character]]) -> Int {
        for row in 0..<board.count {
            for col in 0..<board.first!.count {
                if board[row][col] == "R" {
                    return CaptureType.allCases.reduce(0) {
                        $0 + capture(board, row, col, $1)
                    }
                }
            }
        }
        return 0
    }
    
    func capture(_ board: [[Character]], _ row: Int, _ col: Int, _ type: CaptureType) -> Int {
        var count = 0
        for i in 0..<(type == .col ? board.first!.count : board.count) {
            var exit = false
            switch (type == .col ? board[row][i] : board[i][col]) {
                case "B":
                if i < (type == .col ? col : row) {
                    count = 0
                } else {
                    return count
                }
                case "p":
                if i < (type == .col ? col : row) {
                    count = 1
                } else {
                    count = min(count+1, 2)
                }
                default:
                break
            }
        }
        return count
    }
}
```
