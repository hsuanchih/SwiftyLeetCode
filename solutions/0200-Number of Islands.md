
### Number of Islands

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

__Example 1:__
```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]

Output: 1
```
__Example 2:__
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]

Output: 3
```

__Constraints:__
* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 300`
* `grid[i][j]` is `'0'` or `'1'`.


### Solution
__O(grid) Time:__
```Swift
class Solution {
    func numIslands(_ grid: [[Character]]) -> Int {
        var grid: [[Character]] = grid
        var islands: Int = 0
        for row in 0 ..< grid.count {
            for col in 0 ..< grid.first!.count where grid[row][col] == "1" {
                islands += 1
                markAsWater(&grid, row: row, col: col)
            }
        }
        return islands
    }

    func markAsWater(_ grid: inout [[Character]], row: Int, col: Int) {
        switch (row, col) {
        case (0 ..< grid.count, 0 ..< grid.first!.count) where grid[row][col] == "1":
            grid[row][col] = "0"
            [-1, 1].forEach {
                markAsWater(&grid, row: row + $0, col: col)
                markAsWater(&grid, row: row, col: col + $0)
            }
        default:
            break
        }
    }
}
```