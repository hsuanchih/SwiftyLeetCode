
### Set Matrix Zeroes

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it __in place__.

__Example 1:__

![question_73-0.jpg](../images/question_73-0.jpg)
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

![question_73-1.jpg](../images/question_73-1.jpg)
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
__O(n * m) Time, O(n * m) Space:__
```Swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        var zeros: [[Bool]] = Array(
            repeating: Array(repeating: false, count: matrix.first!.count), 
            count: matrix.count
        )

        for row in 0 ..< matrix.count {
            for col in 0 ..< matrix.first!.count where matrix[row][col] == 0 {
                zeros[row][col] = true
            }
        }

        for row in 0 ..< zeros.count {
            for col in 0 ..< zeros.first!.count where zeros[row][col] {
                for i in 0 ..< matrix.count {
                    matrix[i][col] = 0
                }
                for i in 0 ..< matrix.first!.count {
                    matrix[row][i] = 0
                }
            }
        }
    }
}
```
__O(n * m) Time, O(n + m) Space:__
```Swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        var rowZeros: [Bool] = Array(repeating: false, count: matrix.count)
        var colZeros: [Bool] = Array(repeating: false, count: matrix.first!.count)
        for row in 0 ..< matrix.count {
            for col in 0 ..< matrix.first!.count where matrix[row][col] == 0 {
                rowZeros[row] = true
                colZeros[col] = true
            }
        }

        for row in 0 ..< rowZeros.count where rowZeros[row] {
            for col in 0 ..< matrix.first!.count {
                matrix[row][col] = 0
            }
        }

        for col in 0 ..< colZeros.count where colZeros[col] {
            for row in 0 ..< matrix.count {
                matrix[row][col] = 0
            }
        }
    }
}
```
__O(n * m) Time, O(1) Space:__
```Swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        var isRow0Zero: Bool = false
        var isCol0Zero: Bool = false
        for row in 0 ..< matrix.count {
            for col in 0 ..< matrix.first!.count where matrix[row][col] == 0 {
                switch (row, col) {
                case (0, 0):
                    isRow0Zero = true
                    isCol0Zero = true
                case (0, _):
                    isRow0Zero = true
                case (_, 0):
                    isCol0Zero = true
                case (let row, let col):
                    matrix[0][col] = 0
                    matrix[row][0] = 0
                }
            }
        }

        for row in 1 ..< matrix.count where matrix[row][0] == 0 {
            for col in 0 ..< matrix.first!.count {
                matrix[row][col] = 0
            }
        }

        for col in 1 ..< matrix.first!.count where matrix[0][col] == 0 {
            for row in 0 ..< matrix.count {
                matrix[row][col] = 0
            }
        }

        if isRow0Zero {
            for col in 0 ..< matrix.first!.count {
                matrix[0][col] = 0
            }
        }

        if isCol0Zero {
            for row in 0 ..< matrix.count {
                matrix[row][0] = 0
            }
        }
    }
}
```