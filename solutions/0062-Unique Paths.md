
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
__O(pow(2, m*n)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        paths(m, n, row: 0, col: 0)
    }

    func paths(_ m: Int, _ n: Int, row: Int, col: Int) -> Int {
        switch (row, col) {
        case (m - 1, n - 1):
            return 1
        case (m, _), (_, n):
            return 0
        case (let row, let col):
            return paths(m, n, row: row + 1, col: col) + paths(m, n, row: row, col: col + 1)
        }
    }
}
```
__O(m\*n) Time, O(m\*n) - Memoization:__
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