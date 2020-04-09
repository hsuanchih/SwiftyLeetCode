
### Rotate Image

You are given an *n x n* 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

__Note:__

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly.</br> 
__DO NOT__ allocate another 2D matrix and do the rotation.

__Example 1:__
```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```
__Example 2:__
```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

### Solution
__O(matrix) Time, O(1) Space:__
```Swift
class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        if matrix.isEmpty {
            return
        }

        // Swap along diagnal axis
        for row in 0..<matrix.count {
            for col in row..<matrix.count {
                let temp = matrix[row][col]
                matrix[row][col] = matrix[col][row]
                matrix[col][row] = temp
            }
        }

        // Swap along x-axis
        for row in 0..<matrix.count {
            var start = 0, end = matrix.count-1
            while start < end {
                matrix[row].swapAt(start, end)
                start+=1
                end-=1
            }
        }
    }
}
```