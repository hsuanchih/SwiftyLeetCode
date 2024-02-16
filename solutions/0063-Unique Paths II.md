
### Unique Paths II

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

![Above is a 7 x 3 grid. How many possible unique paths are there?](images/question_62.png)

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

__Note:__ *m* and *n* will be at most 100.

__Example 1:__
```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).
```

### Solution
__O(2^(m*n)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func uniquePathsWithObstacles(_ obstacleGrid: [[Int]]) -> Int {
        return solve(obstacleGrid, 0, 0)
    }
    
    func solve(_ grid: [[Int]], _ row: Int, _ col: Int) -> Int {
        if grid[row][col] == 1 {
            return 0
        }
        switch (row, col) {
            case (grid.count-1, (grid.first?.count ?? 0)-1):
            return 1
            case (grid.count-1, _):
            return solve(grid, row, col+1)
            case (_,  (grid.first?.count ?? 0)-1):
            return solve(grid, row+1, col)
            default:
            return solve(grid, row, col+1) + solve(grid, row+1, col)
        }
    }
}
```
__O(m\*n) Time, O(m\*n) Space - Iterative + Memoization:__
```Swift
class Solution {
    func uniquePathsWithObstacles(_ obstacleGrid: [[Int]]) -> Int {
        guard !obstacleGrid.isEmpty else { return 0 }
        var memo: [[Int]] = Array(repeating: Array(repeating: 0, count: obstacleGrid.first!.count), count: obstacleGrid.count)
        for row in 0 ..< obstacleGrid.count {
            for col in 0 ..< obstacleGrid.first!.count where obstacleGrid[row][col] == 0 {
                switch (row, col) {
                case (0, 0):
                    memo[row][col] = 1
                case (0, let col):
                    memo[row][col] = memo[row][col - 1]
                case (let row, 0):
                    memo[row][col] = memo[row - 1][col]
                case (let row, let col):
                    memo[row][col] = memo[row - 1][col] + memo[row][col - 1]
                }
            }
        }
        return memo.last?.last ?? 0
    }
}
```