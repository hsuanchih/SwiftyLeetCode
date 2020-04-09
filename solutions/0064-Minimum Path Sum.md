
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
__O(2^(m*n)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func minPathSum(_ grid: [[Int]]) -> Int {
        return minSum(grid, 0, 0)
    }
    
    func minSum(_ grid: [[Int]], _ row: Int, _ col: Int) -> Int {
        switch (row, col) {
            case (grid.count-1, (grid.first?.count ?? 0)-1):
            return grid[row][col]
            case (grid.count-1, _):
            return grid[row][col] + minSum(grid, row, col+1)
            case (_, (grid.first?.count ?? 0)-1):
            return grid[row][col] + minSum(grid, row+1, col)
            default:
            return grid[row][col] + min(minSum(grid, row, col+1), minSum(grid, row+1, col))
        }
    }
}
```
__O(m\*n) Time, O(1) Space - Memoization:__
```Swift
class Solution {
    func minPathSum(_ grid: [[Int]]) -> Int {
        var grid = grid
        for row in stride(from: 0, to: grid.count, by: 1) {
            for col in stride(from: 0, to: grid.first!.count, by: 1) {
                switch (row, col) {
                    case (0, 0):
                    break
                    case (0, _):
                    grid[row][col] += grid[row][col-1]
                    case (_, 0):
                    grid[row][col] += grid[row-1][col]
                    default:
                    grid[row][col] += min(grid[row][col-1], grid[row-1][col])
                }
            }
        }
        return grid.last?.last ?? 0
    }
}
```