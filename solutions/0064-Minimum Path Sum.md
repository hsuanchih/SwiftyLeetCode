
### Minimum Path Sum

Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

__Note:__ You can only move either down or right at any point in time.

__Example:__
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```

### Solution
__O(pow(2, m*n)) Time, O(1) Space - Recursive Brute-Force:__
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