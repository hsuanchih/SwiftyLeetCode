
### Number of Enclaves

You are given an `m x n` binary matrix `grid`, where `0` represents a sea cell and `1` represents a land cell.

A __move__ consists of walking from one land cell to another adjacent (__4-directionally__) land cell or walking off the boundary of the `grid`.

Return _the number of land cells in_ `grid` _for which we cannot walk off the boundary of the grid in any number of_ ***moves***.

__Example 1:__

![images/question_1020-1.jpg](images/question_1020-1.jpg)

```
Input: grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
Output: 3
Explanation: There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.
```

__Example 2:__

![images/question_1020-2.jpg](images/question_1020-2.jpg)

```
Input: grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
Output: 0
Explanation: All 1s are either on the boundary or can reach the boundary.
```

__Constraints:__
* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 500`
* `grid[i][j]` is either `0` or `1`.

### Solution
__Brute Force - TLE:__
```Swift
class Solution {
    func numEnclaves(_ grid: [[Int]]) -> Int {
        var grid: [[Int]] = grid
        var cells: Int = 0
        var result: Int = 0
        for row in 0 ..< grid.count {
            for col in 0 ..< (grid.first?.count ?? 0) {
                if grid[row][col] == 1, !canReachBorder(grid: &grid, row: row, col: col) {
                    result += 1
                }
            }
        }
        return result
    }

    func canReachBorder(grid: inout [[Int]], row: Int, col: Int) -> Bool {
        guard row >= 0, row < grid.count, col >= 0, col < (grid.first?.count ?? 0) else {
            return true
        }
        if grid[row][col] == 1 {
            grid[row][col] = 0
            for offset in [-1, 1] {
                if canReachBorder(grid: &grid, row: row + offset, col: col) || canReachBorder(grid: &grid, row: row, col: col + offset) {
                    grid[row][col] = 1
                    return true
                }
            }
            grid[row][col] = 1
        }
        return false
    }
}
```

__O(m*n) Time, O(1) Space:__
```Swift
class Solution {
    func numEnclaves(_ grid: [[Int]]) -> Int {
        // Count the number of lands in grid
        let lands: Int = grid.reduce(into: 0) { result, col in
            result += col.reduce(into: 0, +=)
        }

        var grid: [[Int]] = grid
        var landsReachingBorder: Int = 0
        for row in 0 ..< grid.count {
            for col in 0 ..< (grid.first?.count ?? 0) {
                switch (row, col) {
                case (0, _), (grid.count - 1, _), (_, 0), (_, (grid.first?.count ?? 0) - 1):
                    // Start from borders & count the number of lands reachable
                    landsReachingBorder += landsReachable(grid: &grid, row: row, col: col)
                default:
                    break
                }
            }
        }
        return lands - landsReachingBorder
    }

    func landsReachable(grid: inout [[Int]], row: Int, col: Int) -> Int {
        guard row >= 0, row < grid.count, col >= 0, col < (grid.first?.count ?? 0), grid[row][col] == 1 else {
            return 0
        }
        grid[row][col] = 0
        var lands: Int = 1
        [-1, 1].forEach {
            lands += landsReachable(grid: &grid, row: row + $0, col: col)
            lands += landsReachable(grid: &grid, row: row, col: col + $0)
        }
        return lands
    }
}
```