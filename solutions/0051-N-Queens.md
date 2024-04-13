
### N-Queens

The *n*-queens puzzle is the problem of placing *n* queens on an *n×n* chessboard such that no two queens attack each other.

![One solution to the 8 queens puzzle](../images/question_51.png)

Given an integer *n*, return all distinct solutions to the *n*-queens puzzle.

Each solution contains a distinct board configuration of the *n*-queens' placement,</br> 
where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

__Example:__
```
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```

### Solution
__O(pow(n, 2)) Space:__
```Swift
class Solution {
    func solveNQueens(_ n: Int) -> [[String]] {
        var board: [[Bool]] = Array(repeating: Array(repeating: false, count: n), count: n)
        var result: [[String]] = []
        solve(0, &board, &result)
        return result
    }

    func solve(_ row: Int, _ board: inout [[Bool]], _ result: inout [[String]]) {
        if row == board.count {
            result.append(
                board.map { row in
                    row.reduce(into: "") { result, col in
                        result += (col ? "Q" : ".")
                    }
                }
            )
        } else {
            for col in 0 ..< board.count where row == 0 || isValid(board, row, col) {
                board[row][col] = true
                solve(row + 1, &board, &result)
                board[row][col] = false
            }
        }
    }

    func isValid(_ board: [[Bool]], _ row: Int, _ col: Int) -> Bool {
        for i in 1 ... row where (col - i >= 0 && board[row - i][col - i]) || (col + i < board.count && board[row - i][col + i]) || board[row - i][col] {
            return false
        }
        return true
    }
}
```
__O(n) Space:__
```Swift
class Solution {
    func solveNQueens(_ n: Int) -> [[String]] {
        var queenPlacement: [Int] = Array(repeating: 0, count: n)
        var result: [[String]] = []
        solve(0, &queenPlacement, &result)
        return result
    }

    func solve(_ row: Int, _ queenPlacement: inout [Int], _ result: inout [[String]]) {
        if row == queenPlacement.count {
            result.append(
                queenPlacement.map { row in
                    (0 ..< queenPlacement.count).reduce(into: "") { result, col in
                        result += (col == row ? "Q" : ".")
                    }
                }
            )
        } else {
            for col in 0 ..< queenPlacement.count where row == 0 || isValid(queenPlacement, row, col) {
                queenPlacement[row] = col
                solve(row + 1, &queenPlacement, &result)
                queenPlacement[row] = 0
            }
        }
    }

    func isValid(_ queenPlacement: [Int], _ row: Int, _ col: Int) -> Bool {
        for i in 1 ... row where queenPlacement[row - i] == col - i || queenPlacement[row - i] == col + i || queenPlacement[row - i] == col {
            return false
        }
        return true
    }
}
```