
### Unique Paths

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![Above is a 7 x 3 grid. How many possible unique paths are there?](images/question_62.png)

__Note:__ *m* and *n* will be at most 100.

__Example 1:__
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```
__Example 2:__
```
Input: m = 7, n = 3
Output: 28
```

### Solution
__O(2^(m*n)):__
```Swift
class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        return solve(n, m, 0, 0)
    }
    
    func solve(_ n: Int, _ m: Int, _ row: Int, _ col: Int) -> Int {
        switch (row, col) {
            case (n-1, m-1):
            return 1
            case (n-1, _):
            return solve(n, m, row, col+1)
            case (_, m-1):
            return solve(n, m, row+1, col)
            default:
            return solve(n, m, row, col+1) + solve(n, m, row+1, col)
        }
    }
}
```
__O(m*n):__
```Swift
class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        var memo : [[Int]] = Array(repeating: Array(repeating: 1, count: m), count: n)
        for row in stride(from: 1, to: n, by: 1) {
            for col in stride(from: 1, to: m, by: 1) {
                memo[row][col] = memo[row-1][col] + memo[row][col-1]
            }
        }
        return memo.last?.last ?? 1
    }
}
```