
### Spiral Matrix

Given an `m x n` `matrix`, return all elements of the `matrix` in spiral order.

__Example 1:__

![question_54-0.jpg](../images/question_54-0.jpg)
```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```
__Example 2:__

![question_54-1.jpg](../images/question_54-1.jpg)
```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

__Constraints:__
* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= m, n <= 10`
* `-100 <= matrix[i][j] <= 100`

### Solution
__O(matrix) Time:__
```Swift
class Solution {
    func spiralOrder(_ matrix: [[Int]]) -> [Int] {
        var rowStart: Int = 0
        var rowEnd: Int = matrix.count - 1
        var colStart: Int = 0
        var colEnd: Int = matrix.first!.count - 1
        var result: [Int] = []

        while rowStart <= rowEnd, colStart <= colEnd {
            for col in colStart ... colEnd {
                result.append(matrix[rowStart][col])
            }
            if rowStart < rowEnd {
                for row in rowStart + 1 ... rowEnd {
                    result.append(matrix[row][colEnd])
                }
                for col in stride(from: colEnd - 1, through: colStart, by: -1) {
                    result.append(matrix[rowEnd][col])
                }
            }
            if colStart < colEnd {
                for row in stride(from: rowEnd - 1, to: rowStart, by: -1) {
                    result.append(matrix[row][colStart])
                }
            }
            rowStart += 1
            rowEnd -= 1
            colStart += 1
            colEnd -= 1
        }
        return result
    }
}
```