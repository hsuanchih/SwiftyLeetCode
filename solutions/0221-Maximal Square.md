
### Maximal Square

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

__Example:__
```
Input: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output: 4
```

### Solution
__O(matrix) Time, O(matrix) Space:__
```Swift
class Solution {
    func maximalSquare(_ matrix: [[Character]]) -> Int {
        var memo : [[Int]] = Array(repeating: Array(repeating: 0, count: matrix.first?.count ?? 0), count: matrix.count), result = 0
        for row in 0..<matrix.count {
            for col in 0..<(matrix.first?.count ?? 0) {
                if matrix[row][col] == "1" {
                    switch (row, col) {
                    case (0, _), (_, 0):
                        memo[row][col] = 1
                    default:
                        memo[row][col] = min(min(memo[row][col-1], memo[row-1][col]), memo[row-1][col-1]) + 1
                    }
                    result = max(result, memo[row][col])
                }
            }
        }
        return result*result
    }
}
```