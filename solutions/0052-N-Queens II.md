
### N-Queens II

The *n*-queens puzzle is the problem of placing *n* queens on an *n×n* chessboard such that no two queens attack each other.

![One solution to the 8 queens puzzle](images/question_51.png)

Given an integer *n*, return the number of solutions to the *n*-queens puzzle.

__Example:__
```
Input: 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown below.
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

### Solution
__Recursive:__
```Swift
class Solution {
    func totalNQueens(_ n: Int) -> Int {
        var queenPlacement: [Int] = Array(repeating: 0, count: n)
        var result: Int = 0
        solve(0, &queenPlacement, &result)
        return result
    }

    func solve(_ row: Int, _ queenPlacement: inout [Int], _ result: inout Int) {
        if row == queenPlacement.count {
            result += 1
        } else {
            for col in 0 ..< queenPlacement.count where row == 0 || isValid(queenPlacement, row, col) {
                queenPlacement[row] = col
                solve(row + 1, &queenPlacement, &result)
                queenPlacement[row] = 0
            }
        }
    }

    func isValid(_ queenPlacement: [Int], _ row: Int, _ col: Int) -> Bool {
        for i in 1 ... row where queenPlacement[row - i] == col || queenPlacement[row - i] == col - i || queenPlacement[row - i] == col + i {
            return false
        }
        return true
    }
}
```
