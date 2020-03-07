
### Range Sum Query 2D - Immutable

Given a 2D matrix *matrix*, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

Range Sum Query 2D
The above rectangle (with the red border) is defined by (row1, col1) = __(2, 1)__ and (row2, col2) = __(4, 3)__, which contains sum = __8__.

__Example:__
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

__Note:__
1. You may assume that the matrix does not change.
2. There are many calls to *sumRegion* function.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.

### Solution
```Swift
class NumMatrix {
    
    // memo[row][col] stores the cumulative sum of the all elements within region
    // matrix[0...row-1][0...col-1]
    var memo : [[Int]] = []

    init(_ matrix: [[Int]]) {
        guard !matrix.isEmpty else { return }
        let rows = matrix.count, cols = matrix.first?.count ?? 0
        memo = Array(repeating: Array(repeating: 0, count: cols+1), count: rows+1)
        for row in 1...rows {
            for col in 1...cols {
                memo[row][col] = sumRegion(0, 0, row-1, col-2) + sumRegion(0, 0, row-2, col-1) - sumRegion(0, 0, row-2, col-2) + matrix[row-1][col-1]
            }
        }
    }
    
    func sumRegion(_ row1: Int, _ col1: Int, _ row2: Int, _ col2: Int) -> Int {
        switch (row1, col1) {
            case (0, 0):
            return memo[row2+1][col2+1]
            default:
            return sumRegion(0, 0, row2, col2) - 
            sumRegion(0, 0, row2, col1-1) - 
            sumRegion(0, 0, row1-1, col2) + 
            sumRegion(0, 0, row1-1, col1-1)
        }
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * let obj = NumMatrix(matrix)
 * let ret_1: Int = obj.sumRegion(row1, col1, row2, col2)
 */
```