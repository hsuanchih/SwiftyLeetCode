
### Maximal Square

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, find the largest square containing only `1`'s and return its area.

__Example 1:__

![question_221-0.jpg](../images/question_221-0.jpg)
```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
```
__Example 2:__

![question_221-1.jpg](../images/question_221-1.jpg)
```
Input: matrix = [["0","1"],["1","0"]]
Output: 1
```
__Example 3:__
```
Input: matrix = [["0"]]
Output: 0
```

__Constraints:__
* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= m, n <= 300`
* `matrix[i][j]` is `'0'` or `'1'`.

### Solution
__O(pow(matrix, 2)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func maximalSquare(_ matrix: [[Character]]) -> Int {
        guard !matrix.isEmpty else { return 0 }
        var maxWidth: Int = 0
        for row in 0 ..< matrix.count {
            for col in 0 ..< matrix.first!.count where matrix[row][col] == "1" {
                maxWidth = max(maxWidth, squareWidth(matrix, row, col))
            }
        }
        return maxWidth * maxWidth
    }

    func squareWidth(_ matrix: [[Character]], _ row: Int, _ col: Int) -> Int {
        let maxWidth: Int = min(matrix.count - row, matrix.first!.count - col)
        for width in 0 ..< maxWidth {
            for i in 0 ... width {
                for j in 0 ... width where matrix[row + i][col + j] == "0" {
                    return width
                }
            }
        }
        return maxWidth
    }
}
```
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