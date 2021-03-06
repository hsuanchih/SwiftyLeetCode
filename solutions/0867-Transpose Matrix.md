
### Transpose Matrix

Given a matrix `A`, return the transpose of `A`.

The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.

![Example](images/question_867.png)

__Example 1:__
```
Input: [[1,2,3],[4,5,6],[7,8,9]]
Output: [[1,4,7],[2,5,8],[3,6,9]]
```
__Example 2:__
```
Input: [[1,2,3],[4,5,6]]
Output: [[1,4],[2,5],[3,6]]
```

__Note:__
1. `1 <= A.length <= 1000`
2. `1 <= A[0].length <= 1000`

### Solution
__O(A) Time, O(A) Space:__
```Swift
class Solution {
    func transpose(_ A: [[Int]]) -> [[Int]] {
        if A.isEmpty || A.first!.isEmpty {
            return A
        }
        var result : [[Int]] = Array(repeating: Array(repeating: 0, count: A.count), count: A.first!.count)
        for row in 0 ..< A.count {
            for col in 0 ..< A.first!.count {
                result[col][row] = A[row][col]
            }
        }
        return result
    }
}
```
