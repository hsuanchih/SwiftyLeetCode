
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
        var placement : [Int] = Array(repeating: 0, count: n), result : Int = 0
        solve(&placement, 0, &result)
        return result
    }
    
    func solve(_ placement: inout [Int], _ row: Int, _ result: inout Int) {
        if row == placement.count {
            result+=1
            return
        }
        for col in 0..<placement.count {
            if isValid(placement, row, col) {
                placement[row] = col
                solve(&placement, row+1, &result)
            }
        }
    }
    
    func isValid(_ placement: [Int], _ row: Int, _ col: Int) -> Bool {
        for r in stride(from: row-1, through: 0, by: -1) {
            switch placement[r] {
                case col, col-(row-r), col+row-r:
                return false
                default:
                break
            }
        }
        return true
    }
}
```
