
### Toeplitz Matrix

A matrix is *Toeplitz* if every diagonal from top-left to bottom-right has the same element.

Now given an `M x N` matrix, return `True` if and only if the matrix is *Toeplitz*.

__Example 1:__
```
Input:
matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
]
Output: True
Explanation:
In the above grid, the diagonals are:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
In each diagonal all elements are the same, so the answer is True.
```
__Example 2:__
```
Input:
matrix = [
  [1,2],
  [2,2]
]
Output: False
Explanation:
The diagonal "[1, 2]" has different elements.
```

__Note:__
1. `matrix` will be a 2D array of integers.
2. `matrix` will have a number of rows and columns in range `[1, 20]`.
3. `matrix[i][j]` will be integers in range `[0, 99]`.

__Follow up:__
1. What if the matrix is stored on disk, and the memory is limited such that you can only load at most one row of the matrix into the memory at once?
2. What if the matrix is so large that you can only load up a partial row into the memory at once?

### Solution
__O(m*n) Recursive:__
```Swift
class Solution {
    func isToeplitzMatrix(_ matrix: [[Int]]) -> Bool {
        for col in stride(from: 0, to: matrix.first?.count ?? 0, by: 1) {
            if !isSameValue(matrix[0][col], matrix, 0, col) {
                return false
            }
        }
        for row in stride(from: 1, to: matrix.count, by: 1) {
            if !isSameValue(matrix[row][0], matrix, row, 0) {
                return false
            }
        }
        return true
    }
    
    func isSameValue(_ value: Int, _ matrix: [[Int]], _ row: Int, _ col: Int) -> Bool {
        switch (row, col) {
            case (0..<matrix.count, 0..<matrix.first!.count):
            if matrix[row][col] == value {
                return isSameValue(value, matrix, row+1, col+1)
            }
            return false
            default:
            return true
        }
    }
}
```
__O(m*n) Iterative:__
```Swift
class Solution {
    func isToeplitzMatrix(_ matrix: [[Int]]) -> Bool {
        for index in stride(from: 0, to: matrix.first?.count ?? 0, by: 1) {
            if !isSameValue(matrix, 0, index) {
                return false
            }
        }
        for index in stride(from: 0, to: matrix.count, by: 1) {
            if !isSameValue(matrix, index, 0) {
                return false
            }
        }
        return true
    }
    
    func isSameValue(_ matrix: [[Int]], _ row: Int, _ col: Int) -> Bool {
        var row = row, col = col
        let value = matrix[row][col]
        while row < matrix.count && col < matrix.first?.count ?? 0 {
            if matrix[row][col] != value {
                return false
            }
            row+=1
            col+=1
        }
        return true
    }
}
```