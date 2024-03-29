
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
__DFS from every water cell, TLE:__
```swift
class Solution {
    func maxDistance(_ grid: [[Int]]) -> Int {
        var grid: [[Int]] = grid
        let rows: Int = grid.count, cols: Int = grid.first?.count ?? 0
        var result: Int = 0
        for row in 0 ..< rows {
            for col in 0 ..< cols {
                if grid[row][col] == 1 {
                    grid[row][col] = 0
                } else {
                    grid[row][col] = 1
                }
            }
        }
        for row in 0 ..< rows {
            for col in 0 ..< cols {
                let distance: Int = distance(grid: &grid, row: row, col: col, rows: rows, cols: cols)
                grid[row][col] = distance
                if distance != Int.max {
                    result = max(result, distance)
                }
            }
        }
        return result == 0 ? -1: result
    }

    func distance(grid: inout [[Int]], row: Int, col: Int, rows: Int, cols: Int) -> Int {
        guard row >= 0, row < rows, col >= 0, col < cols else { return Int.max }
        if grid[row][col] == 0 {
            return 0
        } else if grid[row][col] == 1 {
            grid[row][col] = Int.max
            var minDistance: Int = Int.max
            [-1, 1].forEach {
                minDistance = min(
                    minDistance,
                    distance(grid: &grid, row: row + $0, col: col, rows: rows, cols: cols)
                )
                minDistance = min(
                    minDistance,
                    distance(grid: &grid, row: row, col: col + $0, rows: rows, cols: cols)
                )
            }
            grid[row][col] = 1
            return minDistance == Int.max ? Int.max : minDistance + 1
        } else {
            return grid[row][col]
        }
    }
}
```

__O(4^(n*n)), BFS from every water cell, TLE:__
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

__O(2*(n*n)), BFS approaching from all land cells:__
```swift
class Solution {
    struct Cell: Hashable {
        let row: Int, col: Int
    }
    
    func maxDistance(_ grid: [[Int]]) -> Int {
        guard !grid.isEmpty else { return -1 }
        // A value one represents a land/visited cell
        var grid = grid, queue: [Cell] = []
        var levels: Int = -1
        (0 ..< grid.count).forEach { row in
            (0 ..< grid.first!.count).forEach { col in
                // Add all land cells into the initial queue for BFS
                if grid[row][col] == 1 {
                    queue.append(Cell(row: row, col: col))
                }
            }
        }
        
        while !queue.isEmpty {
            let count = queue.count
            for _ in 0 ..< count {
                let current = queue.removeFirst()
                [-1, 1].forEach {
                    [
                        Cell(row: current.row + $0, col: current.col), 
                        Cell(row: current.row, col: current.col + $0),
                    ].forEach { cell in
                        switch (cell.row, cell.col) {
                        case (0 ..< grid.count, 0 ..< grid.first!.count) where grid[cell.row][cell.col] != 1:
                            // Add any water cell that's not previously visited to the queue for BFS, and mark it as visited
                            grid[cell.row][cell.col] = 1
                            queue.append(cell)
                        default:
                            break
                        }
                    }
                }
            }
            levels += 1
        }
        
        // If levels is -1, there are no land cells
        // If levels is 0, there are no water cells
        // Otherwise, return the max BFS level that can be reached approaching from all land cells simultaneously
        return levels <= 0 ? -1 : levels
    }
}
```