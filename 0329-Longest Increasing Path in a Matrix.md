
### Longest Increasing Path in a Matrix

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).

__Example 1:__
```
Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].
```
__Example 2:__
```
Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

### Solution
__O((n*m)^2):__
```Swift
class Solution {
    func longestIncreasingPath(_ matrix: [[Int]]) -> Int {
        if matrix.isEmpty { return 0 }
        var result = 0
        for row in 0..<matrix.count {
            for col in 0..<matrix.first!.count {
                result = max(result, findLongest(matrix, Int.min, row, col))
            }
        }
        return result
    }
    
    func findLongest(_ matrix: [[Int]], _ prev: Int, _ row: Int, _ col: Int) -> Int {
        switch (row, col) {
            case (0..<matrix.count, 0..<matrix.first!.count):
            let curr = matrix[row][col]
            var longest = 0
            if curr > prev {
                for index in [-1, 1] {
                    longest = max(
                        max(longest, findLongest(matrix, curr, row+index, col)), 
                        findLongest(matrix, curr, row, col+index)
                    )
                }
                return longest+1
            }
            fallthrough
            default:
            return 0
        }
    }
}
```
__Amortized O((n\*m)*2):__
```Swift
class Solution {
    func longestIncreasingPath(_ matrix: [[Int]]) -> Int {
        if matrix.isEmpty { return 0 }

        // memo stores the longest increasing path starting from matrix[i,j]
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: matrix.first!.count), count: matrix.count), result = 0
        for row in 0..<matrix.count {
            for col in 0..<matrix.first!.count {
                result = max(result, findLongest(matrix, Int.min, row, col, &memo))
            }
        }
        return result
    }
    
    func findLongest(_ matrix: [[Int]], _ prev: Int, _ row: Int, _ col: Int, _ memo: inout [[Int?]]) -> Int {
        switch (row, col) {
            case (0..<matrix.count, 0..<matrix.first!.count):
            let curr = matrix[row][col]
            var longest = 0
            if curr > prev {
                if let result = memo[row][col] {
                    return result
                }
                for index in [-1, 1] {
                    longest = max(max(longest, findLongest(matrix, curr, row+index, col, &memo)), 
                    findLongest(matrix, curr, row, col+index, &memo))
                }
                memo[row][col] = longest+1
                return memo[row][col]!
            }
            fallthrough
            default:
            return 0
        }
    }
}
```