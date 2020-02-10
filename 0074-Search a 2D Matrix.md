
### Search a 2D Matrix

Write an efficient algorithm that searches for a value in an *m* x *n* matrix.</br>
This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

__Example 1:__
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```
__Example 2:__
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

### Solution
__O(n*m):__
```Swift
class Solution{
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        for row in stride(from: 0, to: matrix.count, by: 1) {
            for col in stride(from: 0, to: matrix.first!.count, by: 1) {
                if matrix[row][col] == target {
                    return true
                }
            }
        }
        return false
    }
}
```
__O(log(n*m)):__
```Swift
class Solution {
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        let rows = matrix.count, cols = matrix.first?.count ?? 0
        if rows == 0 || cols == 0 {
            return false
        }
        var start = 0, end = rows*cols-1
        while start+1 < end {
            let mid = start + (end-start)/2
            switch matrix[mid/cols][mid%cols] {
                case target:
                return true
                case Int.min..<target:
                start = mid
                default:
                end = mid
            }
        }
        if matrix[start/cols][start%cols] == target || matrix[end/cols][end%cols] == target {
            return true
        }
        return false
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