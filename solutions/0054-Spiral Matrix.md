
### Spiral Matrix

Given a matrix of *m* x *n* elements (*m* rows, *n* columns), return all elements of the matrix in spiral order.

__Example 1:__
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
```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

### Solution
__O(matrix):__
```Swift
class Solution {
    func spiralOrder(_ matrix: [[Int]]) -> [Int] {
        var result : [Int] = []
        var rowStart = 0, rowEnd = matrix.count-1,
        colStart = 0, colEnd = (matrix.first?.count ?? 0)-1
        while rowStart <= rowEnd && colStart <= colEnd {
            for col in stride(from: colStart, through: colEnd, by: 1) {
                result.append(matrix[rowStart][col])
            }
            for row in stride(from: rowStart+1, through: rowEnd, by: 1) {
                result.append(matrix[row][colEnd])
            }
            if rowStart < rowEnd {
                for col in stride(from: colEnd-1, through: colStart, by: -1) {
                    result.append(matrix[rowEnd][col])
                }
            }
            if colStart < colEnd {
                for row in stride(from: rowEnd-1, through: rowStart+1, by: -1) {
                    result.append(matrix[row][colStart])
                }
            }
            rowStart+=1
            rowEnd-=1
            colStart+=1
            colEnd-=1
        }
        return result
    }
}
```