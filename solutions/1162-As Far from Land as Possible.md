
### As Far from Land as Possible

Given an `n x n` grid containing only values `0` and `1`, where `0` represents water and `1` represents land, find a water cell such that its distance to the nearest land cell is maximized, and return the distance. If no land or water exists in the grid, return `-1.`

The distance used in this problem is the Manhattan distance: the distance between two cells `(x0, y0)` and `(x1, y1)` is `|x0 - x1| + |y0 - y1|`.

__Example 1:__

![images/question_1162-1.jpg](images/question_1162-1.jpg)

```
Input: grid = [
    [1,0,1],
    [0,0,0],
    [1,0,1]
]

Output: 2

Explanation: The cell (1, 1) is as far as possible from all the land with distance 2.
```

__Example 2:__

![images/question_1162-2.jpg](images/question_1162-2.jpg)

```
Input: grid = [
    [1,0,0],
    [0,0,0],
    [0,0,0]
]

Output: 4

Explanation: The cell (2, 2) is as far as possible from all the land with distance 4.
```

__Constraints:__
* `n == grid.length`
* `n == grid[i].length`
* `1 <= n <= 100`
* `grid[i][j]` is `0` or `1`

### Solution
__O(4^(n*n)), BFS TLE:__
```swift
class Solution {
    struct Cell: Hashable {
        let row: Int, col: Int
    }
    
    func maxDistance(_ grid: [[Int]]) -> Int {
        guard !grid.isEmpty else { return -1 }
        var maxDistance: Int = -1
        (0 ..< grid.count).forEach { row in
            (0 ..< grid.first!.count).forEach { col in
                // If the cell is water, find the distance from the water cell to its closest land.
                // Maximize the distance from each cell to its closest land.
                if grid[row][col] == 0 {
                    maxDistance = max(maxDistance, shortestDistance(row: row, col: col, grid: grid))
                }
            }
        }
        return maxDistance
    }
    
    // Use BFS to locate the land closest to a water cell
    func shortestDistance(row: Int, col: Int, grid: [[Int]]) -> Int {
        var queue: [Cell] = [Cell(row: row, col: col)]
        var visited: Set<Cell> = Set(queue)
        var levels: Int = 0
        while !queue.isEmpty {
            let count = queue.count
            for _ in 0 ..< count {
                let current = queue.removeFirst()
                if grid[current.row][current.col] == 1 {
                    return levels
                }
                [-1, 1].forEach {
                    let nextVCell = Cell(row: current.row+$0, col: current.col)
                    if nextVCell.row >= 0, nextVCell.row < grid.count, !visited.contains(nextVCell) {
                        queue.append(nextVCell)
                        visited.insert(nextVCell)
                    }
                    let nextHCell = Cell(row: current.row, col: current.col+$0)
                    if nextHCell.col >= 0, nextHCell.col < grid.first!.count, !visited.contains(nextHCell) {
                        queue.append(nextHCell)
                        visited.insert(nextHCell)
                    }
                }
            }
            levels += 1
        }
        return -1
    }
}
```