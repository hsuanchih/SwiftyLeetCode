
### Minimum Falling Path Sum

Given a __square__ array of integers `A`, we want the __minimum__ sum of a *falling path* through `A`.

A falling path starts at any element in the first row, and chooses one element from each row.  The next row's choice must be in a column that is different from the previous row's column by at most one.

__Example 1:__
```
Input: [[1,2,3],[4,5,6],[7,8,9]]
Output: 12
Explanation: 
The possible falling paths are:
```
* `[1,4,7], [1,4,8], [1,5,7], [1,5,8], [1,5,9]`
* `[2,4,7], [2,4,8], [2,5,7], [2,5,8], [2,5,9], [2,6,8], [2,6,9]`
* `[3,5,7], [3,5,8], [3,5,9], [3,6,8], [3,6,9]`
The falling path with the smallest sum is `[1,4,7]`, so the answer is `12`.

__Note:__
1. `1 <= A.length == A[0].length <= 100`
2. `-100 <= A[i][j] <= 100`

### Solution
__O(A) Time, O(1) Space:__
```Swift
class Solution {
    func minFallingPathSum(_ A: [[Int]]) -> Int {
        var A = A
        for row in 0..<A.count {
            if row == 0 { continue }
            for col in 0..<A.count {
                switch col {
                    case 0:
                    A[row][col] += min(A[row-1][col], A[row-1][col+1])
                    case A.count-1:
                    A[row][col] += min(A[row-1][col-1], A[row-1][col])
                    default:
                    A[row][col] += min(min(A[row-1][col-1], A[row-1][col]), A[row-1][col+1])
                }
            }
        }
        return A.last!.min()!
    }
}
```