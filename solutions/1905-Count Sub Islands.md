
### Count Sub Islands

You are given two `m x n` binary matrices `grid1` and `grid2` containing only `0`'s (representing water) and `1`'s (representing land). An __island__ is a group of `1`'s connected __4-directionally__ (horizontal or vertical). Any cells outside of the grid are considered water cells.

An island in `grid2` is considered a __sub-island__ if there is an island in grid1 that contains __all__ the cells that make up __this__ island in `grid2`.

Return the ***number*** _of islands in_ `grid2` _that are considered_ ***sub-islands***.

__Example 1:__

![images/question_1905-1.png](images/question_1905-1.png)

```
Input: 
grid1 = [
    [1,1,1,0,0],
    [0,1,1,1,1],
    [0,0,0,0,0],
    [1,0,0,0,0],
    [1,1,0,1,1]
], 
grid2 = [
    [1,1,1,0,0],
    [0,0,1,1,1],
    [0,1,0,0,0],
    [1,0,1,1,0],
    [0,1,0,1,0]
]

Output: 3

Explanation: In the picture above, the grid on the left is grid1 and the grid on the right is grid2.
The 1s colored red in grid2 are those considered to be part of a sub-island. There are three sub-islands.
```

__Example 2:__

![images/question_1905-2.png](images/question_1905-2.png)

```
Input:
grid1 = [
    [1,0,1,0,1],
    [1,1,1,1,1],
    [0,0,0,0,0],
    [1,1,1,1,1],
    [1,0,1,0,1]
],
grid2 = [
    [0,0,0,0,0],
    [1,1,1,1,1],
    [0,1,0,1,0],
    [0,1,0,1,0],
    [1,0,0,0,1]
]

Output: 2 

Explanation: In the picture above, the grid on the left is grid1 and the grid on the right is grid2.
The 1s colored red in grid2 are those considered to be part of a sub-island. There are two sub-islands.
```

__Constraints:__
* `m == grid1.length == grid2.length`
* `n == grid1[i].length == grid2[i].length`
* `1 <= m, n <= 500`
* `grid1[i][j]` and `grid2[i][j]` are either `0` or `1`.

### Solution
__O(m^2*n^2) Time:__
```Swift
class Solution {
    func countSubIslands(_ grid1: [[Int]], _ grid2: [[Int]]) -> Int {
        guard !grid1.isEmpty, !grid2.isEmpty else { return 0 }
        let m = grid1.count, n = grid1.first!.count
        var grid2 = grid2, subIslands: Int = 0
        (0 ..< m).forEach { row in
            (0 ..< n).forEach { col in
                if grid1[row][col] == 1 && grid2[row][col] == 1 {
                    subIslands += consolidateIsland(grid1: grid1, grid2: &grid2, row: row, col: col) ? 1 : 0
                }
            }
        }
        return subIslands
    }
    
    func consolidateIsland(grid1: [[Int]], grid2: inout [[Int]], row: Int, col: Int) -> Bool {
        switch (row, col) {
        case (0 ..< grid1.count, 0 ..< grid1.first!.count) where grid2[row][col] == 1:
            if grid1[row][col] == 1 {
                // If grid1[row][col] is also land when grid2[row][col] is land, 
                // then the island this land is part of on grid2 is currently still a sub-island of grid1.
                // Continue checking this land's neighbors to see if this island on grid2 can be grid1's sub-island.
                var isSubIsland: Bool = true
                grid2[row][col] = 0
                [-1, 1].forEach {
                    isSubIsland = consolidateIsland(grid1: grid1, grid2: &grid2, row: row+$0, col: col) && isSubIsland
                    isSubIsland = consolidateIsland(grid1: grid1, grid2: &grid2, row: row, col: col+$0) && isSubIsland
                }
                return isSubIsland
            } else {
                // If this land on grid2 corresponds to water on grid1, then the island this land is part of is not
                // a sub-island of grid1.
                return false
            }
        default:
            // 1. Any cells outside of the grid are considered water cells.
            // 2. This cell on grid2 is water
            // For these cases the sub-island comparison is irrelevant.
            return true
        }
    }
}
```