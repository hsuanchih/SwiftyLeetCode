
### Set Matrix Zeroes

Given a *m* x *n* matrix, if an element is 0, set its entire row and column to 0. Do it __in-place__.

__Example 1:__
```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```
__Example 2:__
```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```
__Follow up:__

* A straight forward solution using O(*mn*) space is probably a bad idea.
* A simple improvement uses O(*m* + *n*) space, but still not the best solution.
* Could you devise a constant space solution?

### Solution
__O(n*m) Time, O(n+m) Space:__
```Swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        var rows : Set<Int> = [], cols : Set<Int> = []
        for row in stride(from: 0, to: matrix.count, by: 1) {
            for col in stride(from: 0, to: matrix.first!.count, by: 1) {
                if matrix[row][col] == 0 {
                    rows.insert(row)
                    cols.insert(col)
                }
            }
        }
        for row in stride(from: 0, to: matrix.count, by: 1) {
            if rows.contains(row) {
                for col in stride(from: 0, to: matrix.first!.count, by: 1) {
                    matrix[row][col] = 0
                }
            }
        }
        for col in stride(from: 0, to: matrix.first!.count, by: 1) {
            if cols.contains(col) {
                for row in stride(from: 0, to: matrix.count, by: 1) {
                    matrix[row][col] = 0
                }
            }
        }
    }
}
```
__O(n*m) Time, O(1) Space:__
```Swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        var colZero = false
        for row in stride(from: 0, to: matrix.count, by: 1) {
            if matrix[row][0] == 0 {
                colZero = true
            }
            for col in stride(from: 1, to: matrix.first!.count, by: 1) {
                if matrix[row][col] == 0 {
                    matrix[0][col] = 0
                    matrix[row][0] = 0
                }
            }
        }
        for row in stride(from: 1, to: matrix.count, by: 1) {
            for col in stride(from: 1, to: matrix.first!.count, by: 1) {
                if matrix[row][0] == 0 || matrix[0][col] == 0 {
                    matrix[row][col] = 0
                }
            }
        }
        if matrix[0][0] == 0 {
            for col in stride(from: 0, to: matrix.first?.count ?? 0, by: 1) {
                matrix[0][col] = 0
            }
        }
        if colZero {
            for row in stride(from: 0, to: matrix.count, by: 1) {
                matrix[row][0] = 0
            }
        }
    }
}
```