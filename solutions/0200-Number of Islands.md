
### Number of Islands

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

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
        var result: Int = 0
        for row in 0 ..< grid.count {
            for col in 0 ..< grid.first!.count where grid[row][col] == "1" {
                result += 1
                convert(&grid, row, col)
            }
        }
        return result
    }

    func convert(_ grid: inout [[Character]], _ row: Int, _ col: Int) {
        switch (row, col) {
        case (0 ..< grid.count, 0 ..< grid.first!.count) where grid[row][col] == "1":
            grid[row][col] = "0"
            for offset in [-1, 1] {
                convert(&grid, row + offset, col)
                convert(&grid, row, col + offset)
            }
        default:
            break
        }
    }
}
```