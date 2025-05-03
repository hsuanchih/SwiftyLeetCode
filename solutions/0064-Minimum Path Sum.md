
### Minimum Path Sum

Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

__Note:__ 
You can only move either down or right at any point in time.

__Example 1:__

![question_64.jpg](../images/question_64.jpg)
```
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```
__Example 2:__
```
Input: grid = [[1,2,3],[4,5,6]]
Output: 12
```

__Constraints:__
* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 200`
* `0 <= grid[i][j] <= 200`

### Solution
__O(pow(2, grid)) Time, O(1) Space - Recursive Brute-Force:__
```Swift
class Solution {
    func minPathSum(_ grid: [[Int]]) -> Int {
        guard !grid.isEmpty else { return .max }
        return minSum(grid, row: 0, col: 0)
    }

    func minSum(_ grid: [[Int]], row: Int, col: Int) -> Int {
        switch (row, col) {
        case (grid.count - 1, grid.first!.count - 1):
            return grid[row][col]
        case (grid.count, _), (_, grid.first!.count):
            return .max
        case (let row, let col):
            return grid[row][col] + min(minSum(grid, row: row + 1, col: col), minSum(grid, row: row, col: col + 1))
        }
    }
}
```
__O(grid) Time, O(grid) Space - Recursive + Memoization:__
```Swift
class Solution {
    func minPathSum(_ grid: [[Int]]) -> Int {
        guard !grid.isEmpty else { return .max }
        var memo: [[Int?]] = Array(repeating: Array(repeating: nil, count: grid.first!.count), count: grid.count)
        return minSum(grid, row: 0, col: 0, memo: &memo)
    }

    func minSum(_ grid: [[Int]], row: Int, col: Int, memo: inout [[Int?]]) -> Int {
        switch (row, col) {
        case (grid.count - 1, grid.first!.count - 1):
            return grid[row][col]
        case (grid.count, _), (_, grid.first!.count):
            return .max
        case (let row, let col):
            if let result = memo[row][col] {
                return result
            } else {
                memo[row][col] = grid[row][col] + min(minSum(grid, row: row + 1, col: col, memo: &memo), minSum(grid, row: row, col: col + 1, memo: &memo))
                return memo[row][col]!
            }
        }
    }
}
```
__O(m\*n) Time, O(m\*n) Space - Iterative + Memoization:__
```Swift
class Solution {
    func minPathSum(_ grid: [[Int]]) -> Int {
        var memo: [[Int]] = Array(repeating: Array(repeating: 0, count: grid.first!.count), count: grid.count)
        for row in 0 ..< grid.count {
            for col in 0 ..< grid.first!.count {
                switch (row, col) {
                case (0, 0):
                    memo[row][col] = grid[row][col]
                case (0, _):
                    memo[row][col] = grid[row][col] + memo[row][col - 1]
                case (_, 0):
                    memo[row][col] = grid[row][col] + memo[row - 1][col]
                case (let row, let col):
                    memo[row][col] = grid[row][col] + min(memo[row][col - 1], memo[row - 1][col])
                }
            }
        }
        return memo.last?.last ?? 0
    }
}
```
__O(m\*n) Time, O(1) Space - Memoization:__
```Swift
class Solution {
    func minPathSum(_ grid: [[Int]]) -> Int {
        var grid: [[Int]] = grid
        for row in 0 ..< grid.count {
            for col in 0 ..< grid.first!.count {
                switch (row, col) {
                case (0, 0):
                    break
                case (0, _):
                    grid[row][col] += grid[row][col - 1]
                case (_, 0):
                    grid[row][col] += grid[row - 1][col]
                case (let row, let col):
                    grid[row][col] += min(grid[row][col - 1], grid[row - 1][col])
                }
            }
        }
        return grid.last?.last ?? 0
    }
}
```