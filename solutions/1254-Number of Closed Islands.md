
### Number of Closed Islands

Given a 2D `grid` consists of `0`s (land) and `1`s (water).  An island is a maximal 4-directionally connected group of `0`s and a _closed island_ is an island __totally__ (all left, top, right, bottom) surrounded by `1`s.

Return the number of _closed islands_.

__Example 1:__

![images/question_1254-1.png](images/question_1254-1.png)

```
Input: grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
Output: 2
Explanation: 
Islands in gray are closed because they are completely surrounded by water (group of 1s).
```

__Example 2:__

![images/question_1254-2.png](images/question_1254-2.png)

```
Input: grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
Output: 1
```

__Example 3:__

```
Input: grid = [[1,1,1,1,1,1,1],
               [1,0,0,0,0,0,1],
               [1,0,1,1,1,0,1],
               [1,0,1,0,1,0,1],
               [1,0,1,1,1,0,1],
               [1,0,0,0,0,0,1],
               [1,1,1,1,1,1,1]]
Output: 2
```

__Constraints:__
* `1 <= grid.length, grid[0].length <= 100`
* `0 <= grid[i][j] <=1`

### Solution
__O(2\*(m*n)) Time:__
```Swift
class Solution {
    func closedIsland(_ grid: [[Int]]) -> Int {
        guard !grid.isEmpty else { return 0 }
        var grid = grid, closedIslands: Int = 0
        setup(grid: &grid)
        (0 ..< grid.count).forEach { row in
            (0 ..< grid.first!.count).forEach { col in
                if grid[row][col] == 0 {
                    closedIslands += 1
                    resolveLandAsWater(grid: &grid, row: row, col: col)
                }
            }
        }
        return closedIslands
    }
    
    func setup(grid: inout [[Int]]) {
        // Visit the first & last column for each row & turn bordering islands into water
        (0 ..< grid.count).forEach { row in
            [0, grid.first!.count-1].forEach { col in
                if grid[row][col] == 0 {
                    resolveLandAsWater(grid: &grid, row: row, col: col)
                }
            }
        }
        // Visit the first & last row of each column & turn bordering islands into water
        (0 ..< grid.first!.count).forEach { col in
            [0, grid.count-1].forEach { row in
                if grid[row][col] == 0 {
                    resolveLandAsWater(grid: &grid, row: row, col: col)
                }
            }
        }
    }
    
    func resolveLandAsWater(grid: inout [[Int]], row: Int, col: Int) {
        switch (row, col) {
        case (0 ..< grid.count, 0 ..< grid.first!.count) where grid[row][col] == 0:
            grid[row][col] = 1
            [-1, 1].forEach {
                resolveLandAsWater(grid: &grid, row: row+$0, col: col)
                resolveLandAsWater(grid: &grid, row: row, col: col+$0)
            }
        default:
            break
        }
    }
}
```