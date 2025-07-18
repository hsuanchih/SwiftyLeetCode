
### Unique Paths II

You are given an `m x n` integer array `grid`. There is a robot initially located at the __top-left corner__ (i.e., `grid[0][0]`). The robot tries to move to the __bottom-right corner__ (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

An obstacle and space are marked as `1` or `0` respectively in `grid`. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to `2 * pow(10, 9)`.

__Example 1:__

![question_63-0.jpg](../images/question_63-0.jpg)
```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```
__Example 2:__

![question_63-1.jpg](../images/question_63-1.jpg)
```
Input: obstacleGrid = [[0,1],[0,0]]
Output: 1
```

__Constraints:__
* `m == obstacleGrid.length`
* `n == obstacleGrid[i].length`
* `1 <= m, n <= 100`
* `obstacleGrid[i][j]` is `0` or `1`.

### Solution
__O(pow(2, m * n)) Time, O(1) Space - Recursive Brute-Force:__
```Swift
class Solution {
    func uniquePathsWithObstacles(_ obstacleGrid: [[Int]]) -> Int {
        guard !obstacleGrid.isEmpty else { return 0 }
        return walk(obstacleGrid, 0, 0)
    }

    func walk(_ grid: [[Int]], _ row: Int, _ col: Int) -> Int {
        switch (row, col) {
        case (grid.count - 1, grid.first!.count - 1) where grid[row][col] == 0:
            return 1
        case (0 ..< grid.count, 0 ..< grid.first!.count) where grid[row][col] == 0:
            return walk(grid, row + 1, col) + walk(grid, row, col + 1)
        default:
            return 0
        }
    }
}
```
__O(m * n) Time, O(m * n) Space - Recursive + Memoization:__
```Swift
class Solution {
    func uniquePathsWithObstacles(_ obstacleGrid: [[Int]]) -> Int {
        guard !obstacleGrid.isEmpty else { return 0 }
        var paths: [[Int?]] = Array(repeating: Array(repeating: nil, count: obstacleGrid.first!.count), count: obstacleGrid.count)
        return walk(obstacleGrid, 0, 0, &paths)
    }

    func walk(_ grid: [[Int]], _ row: Int, _ col: Int, _ paths: inout [[Int?]]) -> Int {
        switch (row, col) {
        case (grid.count - 1, grid.first!.count - 1) where grid[row][col] == 0:
            return 1
        case (0 ..< grid.count, 0 ..< grid.first!.count) where grid[row][col] == 0:
            if let result = paths[row][col] {
                return result
            } else {
                let result = walk(grid, row + 1, col, &paths) + walk(grid, row, col + 1, &paths)
                paths[row][col] = result
                return result
            }
        default:
            return 0
        }
    }
}
```
__O(m * n) Time, O(m * n) Space - Bottom Up Iterative + Memoization:__
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
__O(m * n) Time, O(m * n) Space - Top Down Iterative + Memoization:__
```Swift
class Solution {
    func uniquePathsWithObstacles(_ obstacleGrid: [[Int]]) -> Int {
        var paths: [[Int]] = Array(repeating: Array(repeating: 0, count: obstacleGrid.first!.count), count: obstacleGrid.count)
        for row in stride(from: obstacleGrid.count - 1, through: 0, by: -1) {
            for col in stride(from: obstacleGrid.first!.count - 1, through: 0, by: -1) where obstacleGrid[row][col] == 0 {
                switch (row, col) {
                    case (obstacleGrid.count - 1, obstacleGrid.first!.count - 1):
                        paths[row][col] = 1
                    case (let row, obstacleGrid.first!.count - 1):
                        paths[row][col] = paths[row + 1][col]
                    case (obstacleGrid.count - 1, let col):
                        paths[row][col] = paths[row][col + 1]
                    case (let row, let col):
                        paths[row][col] = paths[row + 1][col] + paths[row][col + 1]
                }
            }
        }
        return paths.first?.first ?? 0
    }
}
```