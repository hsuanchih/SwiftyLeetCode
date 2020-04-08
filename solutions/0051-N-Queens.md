
### N-Queens

The *n*-queens puzzle is the problem of placing *n* queens on an *n×n* chessboard such that no two queens attack each other.

![One solution to the 8 queens puzzle](images/question_51.png)

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
__Recursive:__
```Swift
class Solution {
    func solveNQueens(_ n: Int) -> [[String]] {
        var placement : [Int] = Array(repeating: 0, count: n), result: [[String]] = []
        solve(&placement, 0, &result)
        return result
    }
    
    // Helper method to solve for queen placement in each row
    func solve(_ placement: inout [Int], _ row: Int, _ result: inout [[String]]) {
        if row == placement.count {
            result.append(constructResult(placement))
            return
        }
        for col in 0..<placement.count {
            if isValid(placement, row, col) {
                placement[row] = col
                solve(&placement, row+1, &result)
            }
        }
    }
    
    // Helper method to validate placement of a queue
    func isValid(_ placement: [Int], _ row: Int, _ col: Int) -> Bool {
        for r in stride(from: row-1, through: 0, by: -1) {
            switch placement[r] {
                case col, col-(row-r), col+(row-r):
                return false
                default:
                break
            }
        }
        return true
    }
    
    // Helper method to construct the board given placement
    func constructResult(_ placement: [Int]) -> [String] {
        var result : [String] = []
        for row in 0..<placement.count {
            var temp = ""
            for col in 0..<placement.count {
                if col == placement[row] {
                    temp.append("Q")
                } else {
                    temp.append(".")
                }
            }
            result.append(temp)
        }
        return result
    }
}
```
