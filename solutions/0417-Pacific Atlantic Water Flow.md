
### Pacific Atlantic Water Flow

There is an `m x n` rectangular island that borders both the __Pacific Ocean__ and __Atlantic Ocean__. The __Pacific Ocean__ touches the island's left and top edges, and the __Atlantic Ocean__ touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix heights where `heights[r][c]` represents the __height above sea level__ of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is __less than or equal to__ the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a ***2D list*** _of grid coordinates_ `result` _where_ `result[i] = [ri, ci]` _denotes that rain water can flow from cell_ `(ri, ci)` _to both the Pacific and Atlantic oceans_.

__Example 1:__

![images/question_417.jpg](images/question_417.jpg)

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
__Brute-Force O(4^mn), TLE:__
```swift
class Solution {
    enum Ocean {
        case atlantic, pacific
    }
    
    func pacificAtlantic(_ heights: [[Int]]) -> [[Int]] {
        guard !heights.isEmpty else { return [] }
        var heights = heights, result: [[Int]] = []
        (0 ..< heights.count).forEach { row in
            (0 ..< heights.first!.count).forEach { col in
                if canReach(.pacific, heights: &heights, row: row, col: col, previous: Int.max), 
                    canReach(.atlantic, heights: &heights, row: row, col: col, previous: Int.max) {
                    result.append([row, col])
                }
            }
        }
        return result
    }
    
    func canReach(_ ocean: Ocean, heights: inout [[Int]], row: Int, col: Int, previous: Int) -> Bool {
        switch (ocean, row, col) {
        case (_, 0 ..< heights.count, 0 ..< heights.first!.count):
            let current = heights[row][col]
            if current == -1 || previous < current {
                return false
            } else {
                heights[row][col] = -1
                let top = canReach(ocean, heights: &heights, row: row-1, col: col, previous: current)
                let left = canReach(ocean, heights: &heights, row: row, col: col-1, previous: current)
                let right = canReach(ocean, heights: &heights, row: row, col: col+1, previous: current)
                let bottom = canReach(ocean, heights: &heights, row: row+1, col: col, previous: current)
                heights[row][col] = current
                return top || left || right || bottom
            }
        case (.atlantic, heights.count, _), 
            (.atlantic, _, heights.first!.count),
            (.pacific, -1, _),
            (.pacific, _, -1):
            return true
        default:
            return false
        }
    }
}
```

__Minor Speed-Up O(4^mn), TLE:__
```swift
class Solution {
    enum Ocean: Hashable {
        case atlantic, pacific
    }
    
    func pacificAtlantic(_ heights: [[Int]]) -> [[Int]] {
        guard !heights.isEmpty else { return [] }
        var heights = heights, result: [[Int]] = []
        var reachability: [Ocean: [[Bool?]]] = [
            .atlantic: Array(repeating: Array(repeating: nil, count: heights.first!.count), count: heights.count),
            .pacific: Array(repeating: Array(repeating: nil, count: heights.first!.count), count: heights.count)
        ]
        (0 ..< heights.count).forEach { row in
            (0 ..< heights.first!.count).forEach { col in
                [Ocean.pacific, .atlantic].forEach {
                    reachability[$0]![row][col] = canReach($0, heights: &heights, row: row, col: col, previous: Int.max, reachability: reachability)
                }
                if reachability[.pacific]![row][col]!, reachability[.atlantic]![row][col]! {
                    result.append([row, col])
                }
            }
        }
        return result
    }
    
    func canReach(_ ocean: Ocean, heights: inout [[Int]], row: Int, col: Int, previous: Int, reachability: [Ocean: [[Bool?]]]) -> Bool {
        switch (ocean, row, col) {
        case (let ocean, 0 ..< heights.count, 0 ..< heights.first!.count):
            let current = heights[row][col]
            if previous < current {
                return false
            } else {
                if let canReach = reachability[ocean]![row][col] {
                    return canReach
                } else {
                    heights[row][col] = Int.max
                    let top = canReach(ocean, heights: &heights, row: row-1, col: col, previous: current, reachability: reachability)
                    let left = canReach(ocean, heights: &heights, row: row, col: col-1, previous: current, reachability: reachability)
                    let right = canReach(ocean, heights: &heights, row: row, col: col+1, previous: current, reachability: reachability)
                    let bottom = canReach(ocean, heights: &heights, row: row+1, col: col, previous: current, reachability: reachability)
                    heights[row][col] = current
                    return top || left || right || bottom
                }
            }
        case (.atlantic, heights.count, _),
            (.atlantic, _, heights.first!.count),
            (.pacific, -1, _),
            (.pacific, _, -1):
            return true
        default:
            return false
        }
    }
}
```

__Move in from Ocean O((m+n) + (m*n)):__
```swift
class Solution {
    enum Ocean: Hashable {
        case atlantic, pacific
    }
    
    func pacificAtlantic(_ heights: [[Int]]) -> [[Int]] {
        guard !heights.isEmpty else { return [] }
        var result: [[Int]] = []
        var reachability: [Ocean: [[Bool]]] = [
            .atlantic: Array(repeating: Array(repeating: false, count: heights.first!.count), count: heights.count),
            .pacific: Array(repeating: Array(repeating: false, count: heights.first!.count), count: heights.count)
        ]

        // Move in from the left (pacific) & right (atlantic), and check whether each cell can be reached from either end.
        (0 ..< heights.count).forEach { row in
            canReach(row: row, col: 0, in: heights, from: .pacific, reachability: &reachability, previous: -1)
            canReach(row: row, col: heights.first!.count-1, in: heights, from: .atlantic, reachability: &reachability, previous: -1)
        }

        // Move in from the top (pacific) & bottom (atlantic), and check whether each cell can be reached from either end.
        (0 ..< heights.first!.count).forEach { col in
            canReach(row: 0, col: col, in: heights, from: .pacific, reachability: &reachability, previous: -1)
            canReach(row: heights.count-1, col: col, in: heights, from: .atlantic, reachability: &reachability, previous: -1)
        }
        
        // Check the reachability of each cell & add the cell to result only if the cell can be reached from both the
        // pacific & atlantic ocean.
        (0 ..< heights.count).forEach { row in
            (0 ..< heights.first!.count).forEach { col in
                if reachability[.pacific]![row][col], reachability[.atlantic]![row][col] {
                    result.append([row, col])
                }
            }
        }
        return result
    }
    
    func canReach(row: Int, col: Int, in heights: [[Int]], from ocean: Ocean, reachability: inout [Ocean: [[Bool]]], previous: Int) {
        switch (row, col) {
        case (0 ..< heights.count, 0 ..< heights.first!.count):
            let height = heights[row][col]
            // A cell can only be reached if the height of the previous cell is less than or equal to the height of the current cell.
            // Also, avoid revisting a visited cell by checking against its reachability status.
            if previous <= height && !reachability[ocean]![row][col] {
                reachability[ocean]![row][col] = true
                [-1, 1].forEach {
                    canReach(row: row+$0, col: col, in: heights, from: ocean, reachability: &reachability, previous: height)
                    canReach(row: row, col: col+$0, in: heights, from: ocean, reachability: &reachability, previous: height)
                }
            } 
        default:
            break
        }
    }
}
```