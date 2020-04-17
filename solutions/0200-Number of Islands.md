
### Number of Islands

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

__Example 1:__
```
Input:
11110
11010
11000
00000

Output: 1
```
__Example 2:__
```
Input:
11000
11000
00100
00011

Output: 3
```

### Solution
```Swift
class Solution {
    func numIslands(_ grid: [[Character]]) -> Int {
        let numRows = grid.count, numCols = grid.first?.count ?? 0
        if numRows == 0 || numCols == 0 {
            return 0
        }
        var input = grid, result = 0
        for row in 0..<numRows {
            for col in 0..<numCols {
                if input[row][col] == "1" {
                    result += 1
                    merge(&input, row, col)
                }
            }
        }
        return result
    }
    
    func merge(_ grid: inout [[Character]], _ row: Int, _ col: Int) {
        switch (row, col) {
            case (0..<grid.count, 0..<grid.first!.count):
            if grid[row][col] == "1" {
                grid[row][col] = "0"
                for i in [-1, 1] {
                    merge(&grid, row+i, col)
                    merge(&grid, row, col+i)
                }
            }
            default:
            break
        }
    }
}
```