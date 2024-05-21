
### Rotate Image

You are given an `n x n` 2D `matrix` representing an image, rotate the image by __90__ degrees (clockwise).

You have to rotate the image __in-place__, which means you have to modify the input 2D matrix directly. __DO NOT__ allocate another 2D matrix and do the rotation.

__Example 1:__

![question_48-0.jpg](../images/question_48-0.jpg)
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

![question_48-1.jpg](../images/question_48-1.jpg)
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

__Constraints:__
* `n == matrix.length == matrix[i].length`
* `1 <= n <= 20`
* `-1000 <= matrix[i][j] <= 1000`

### Solution
__O(matrix) Time, O(1) Space:__
```Swift
class Solution {
    func rotate(_ matrix: inout [[Int]]) {

        // Swap along diagnal axis
        for row in 0 ..< matrix.count {
            for col in row ..< matrix.count {
                let temp: Int = matrix[row][col]
                matrix[row][col] = matrix[col][row]
                matrix[col][row] = temp
            }
        }

        // Swap along x-axis
        for row in 0 ..< matrix.count {
            for col in 0 ... (matrix.count - 1) / 2 {
                matrix[row].swapAt(col, matrix.count - 1 - col)
            }
        }
    }
}
```
__O(matrix) Time, O(1) Space:__
```Swift
class Solution {
    func rotate(_ matrix: inout [[Int]]) {

        // Swap along y-axis
        for row in 0 ... (matrix.count - 1) / 2 {
            for col in 0 ..< matrix.count {
                let temp: Int = matrix[row][col]
                matrix[row][col] = matrix[matrix.count - 1 - row][col]
                matrix[matrix.count - 1 - row][col] = temp
            }
        }

        // Swap along diagnal axis
        for row in 0 ..< matrix.count {
            for col in row ..< matrix.count where row != col {
                let temp: Int = matrix[row][col]
                matrix[row][col] = matrix[col][row]
                matrix[col][row] = temp
            }
        }
    }
}
```