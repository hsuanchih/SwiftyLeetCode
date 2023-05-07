
### Max Area of Island

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected __4-directionally__ (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The __area__ of an island is the number of cells with a value `1` in the island.

Return _the maximum ***area*** of an island in_ `grid`. If there is no island, return `0`.

__Example 1:__

![images/question_695.jpg](images/question_695.jpg)

```
Input: grid = [
    [0,0,1,0,0,0,0,1,0,0,0,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,1,1,0,1,0,0,0,0,0,0,0,0],
    [0,1,0,0,1,1,0,0,1,0,1,0,0],
    [0,1,0,0,1,1,0,0,1,1,1,0,0],
    [0,0,0,0,0,0,0,0,0,0,1,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```

__Example 2:__
```
Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0
```

__Constraints:__
* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 50`
* `grid[i][j] is either 0 or 1`

### Solution
__O(m*n) Time:__
```Swift
class Solution {
    func maxAreaOfIsland(_ grid: [[Int]]) -> Int {
        var grid: [[Int]] = grid
        var maxArea: Int = 0
        for row in 0 ..< grid.count {
            for col in 0 ..< (grid.first?.count ?? 0) {
                maxArea = max(
                    maxArea, 
                    turnIslandIntoWater(grid: &grid, row: row, col: col)
                )
            }
        }
        return maxArea
    }

    func turnIslandIntoWater(grid: inout [[Int]], row: Int, col: Int) -> Int {
        guard row >= 0, row < grid.count, col >= 0, col < (grid.first?.count ?? 0), grid[row][col] == 1 else { return 0 }
        grid[row][col] = 0
        var area: Int = 1
        [-1, 1].forEach {
            area += turnIslandIntoWater(grid: &grid, row: row + $0, col: col)
            area += turnIslandIntoWater(grid: &grid, row: row, col: col + $0)
        }
        return area
    }
}
```