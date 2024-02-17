
### Triangle

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
The minimum path sum from top to bottom is `11` (i.e., 2 + 3 + 5 + 1 = 11).

__Note:__
Bonus point if you are able to do this using only O(*n*) extra space, where *n* is the total number of rows in the triangle.

__Example:__
```
Input: 3
Output: [1,3,3,1]
```
__Follow up:__
Could you optimize your algorithm to use only O(k) extra space?

### Solution
__O(pow(2, triangle)) Time, O(1) Space, Brute-Force Recursive:__
```Swift
class Solution {
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        guard !triangle.isEmpty else { return .min }
        return minSum(triangle, row: 0, col: 0)
    }

    func minSum(_ triangle: [[Int]], row: Int, col: Int) -> Int {
        if row == triangle.count - 1 {
            return triangle[row][col]
        } else {
            return triangle[row][col] + min(minSum(triangle, row: row + 1, col: col), minSum(triangle, row: row + 1, col: col + 1))
        }
    }
}
```
__O(triangle) Time, O(triangle) Space:__
```Swift
class Solution {
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        var triangle = triangle
        for row in stride(from: 1, to: triangle.count, by: 1) {
            for col in stride(from: 0, through: row, by: 1) {
                switch col {
                    case 0:
                    triangle[row][col] += triangle[row-1][col]
                    case row:
                    triangle[row][col] += triangle[row-1][col-1]
                    default:
                    triangle[row][col] += min(triangle[row-1][col], triangle[row-1][col-1])
                }
            }
        }
        return triangle.last?.min() ?? 0
    }
}
```
__O(triangle) Time, O(rows) Space:__
```Swift
class Solution {
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        if triangle.isEmpty {
            return 0
        }
        var colSum : [Int] = Array(repeating: 0, count: triangle.count)
        for row in 0..<triangle.count {
            for col in stride(from: row, through: 0, by: -1) {
                switch (row, col) {
                    case (0, _):
                    colSum[col] = triangle[row][col]
                    case (_, 0):
                    colSum[col] += triangle[row][col]
                    case (_, row):
                    colSum[col] = colSum[col-1] + triangle[row][col]
                    default:
                    colSum[col] = min(colSum[col], colSum[col-1]) + triangle[row][col]
                }
            }
        }
        return colSum.reduce(Int.max) { min($0, $1) }
    }
}
```