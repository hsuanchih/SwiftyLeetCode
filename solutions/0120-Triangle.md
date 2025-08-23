
### Triangle

Given a `triangle` array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.


__Example 1:__
```
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
```
__Example 2:__
```
Input: triangle = [[-10]]
Output: -10
```

__Constraints:__
* `1 <= triangle.length <= 200`
* `triangle[0].length == 1`
* `triangle[i].length == triangle[i - 1].length + 1`
* `-pow(10, 4) <= triangle[i][j] <= pow(10, 4)`

__Follow up:__ 
* Could you do this using only `O(n)` extra space, where `n` is the total number of rows in the triangle?


### Solution
__O(pow(2, triangle)) Time, O(1) Space, Brute-Force Recursive:__
```Swift
class Solution {
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        minSum(0, 0, triangle)
    }

    func minSum(_ row: Int, _ col: Int, _ triangle: [[Int]]) -> Int {
        if row == triangle.count {
            return 0
        } else {
            return min(minSum(row + 1, col, triangle), minSum(row + 1, col + 1, triangle)) + triangle[row][col]
        }
    }
}
```
__O(triangle) Time, O(triangle) Space, Recursive + Memoization:__
```Swift
class Solution {
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        var memo: [[Int?]] = Array(repeating: Array(repeating: nil, count: triangle.count), count: triangle.count)
        return minSum(0, 0, triangle, &memo)
    }

    func minSum(_ row: Int, _ col: Int, _ triangle: [[Int]], _ memo: inout [[Int?]]) -> Int {
        let minPathSum: Int
        if row == triangle.count {
            minPathSum = 0
        } else if let value = memo[row][col] {
            minPathSum = value
        } else {
            minPathSum = min(
                minSum(row + 1, col, triangle, &memo),
                minSum(row + 1, col + 1, triangle, &memo)
            ) + triangle[row][col]
            memo[row][col] = minPathSum
        }
        return minPathSum
    }
}
```
__O(triangle) Time, O(triangle) Space - Top-Down Dynamic Programming:__
```Swift
class Solution {
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        guard !triangle.isEmpty else { return .min }
        var pathSum: [[Int]] = []
        for row in 0 ..< triangle.count {
            var rowSum: [Int] = []
            for col in 0 ..< triangle[row].count {
                switch (row, col) {
                case (0, _):
                    rowSum.append(triangle[row][col])
                case (_, 0):
                    rowSum.append(triangle[row][col] + pathSum[row - 1][col])
                case (_, triangle[row].count - 1):
                    rowSum.append(triangle[row][col] + pathSum[row - 1][col - 1])
                case (let row, let col):
                    rowSum.append(triangle[row][col] + min(pathSum[row - 1][col], pathSum[row - 1][col - 1]))
                }
            }
            pathSum.append(rowSum)
        }
        return pathSum.last.flatMap { $0.min() } ?? 0
    }
}
```
__O(triangle) Time, O(triangle) Space - Top-Down Dynamic Programming:__
```Swift
class Solution {
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        var triangle: [[Int]] = triangle
        for row in 1 ..< triangle.count {
            let cols: Int = triangle[row].count
            for col in 0 ..< cols {
                switch col {
                case 0:
                    triangle[row][col] += triangle[row - 1][col]
                case cols - 1:
                    triangle[row][col] += triangle[row - 1][col - 1]
                case let col:
                    triangle[row][col] += min(triangle[row - 1][col], triangle[row - 1][col - 1])
                }
            }
        }
        if let minPathSum = triangle.last?.min() {
            return minPathSum
        } else {
            fatalError()
        }
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
        var colSum: [Int] = Array(repeating: 0, count: triangle.count)
        for row in 0 ..< triangle.count {
            for col in stride(from: row, through: 0, by: -1) {
                switch (row, col) {
                    case (0, _):
                    colSum[col] = triangle[row][col]
                    case (_, 0):
                    colSum[col] += triangle[row][col]
                    case (_, row):
                    colSum[col] = colSum[col - 1] + triangle[row][col]
                    default:
                    colSum[col] = min(colSum[col], colSum[col - 1]) + triangle[row][col]
                }
            }
        }
        return colSum.reduce(Int.max) { min($0, $1) }
    }
}
```