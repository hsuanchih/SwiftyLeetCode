
### N-Queens II

The __n-queens__ puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return the number of distinct solutions to the __n-queens puzzle__.

__Example 1:__

![question_52.jpg](../images/question_52.jpg)
```
Input: n = 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown.
```
__Example 2:__
```
Input: n = 1
Output: 1
```

__Constraints:__
* `1 <= n <= 9`

### Solution
__Recursive:__
```Swift
class Solution {
    func totalNQueens(_ n: Int) -> Int {
        var queens: [Int] = Array(repeating: .min, count: n)
        var result: Int = 0
        solve(&queens, 0, &result)
        return result
    }

    func solve(_ queens: inout [Int], _ row: Int, _ result: inout Int) {
        if row == queens.count {
            result += 1
        } else {
            for col in 0 ..< queens.count where isValid(queens, row, col) {
                queens[row] = col
                solve(&queens, row + 1, &result)
                queens[row] = .min
            }
        }
    }

    func isValid(_ queens: [Int], _ row: Int, _ col: Int) -> Bool {
        for r in 0 ..< row where queens[r] == col || queens[r] == col + (row - r) || queens[r] == col - (row - r) {
            return false
        }
        return true
    }
}
```
