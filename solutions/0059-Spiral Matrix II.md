
### Spiral Matrix II

Given a positive integer *n*, generate a square matrix filled with elements from 1 to *n^2* in spiral order.

__Example:__
```
Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

### Solution
__O(pow(n, 2)) Time:__
```Swift
class Solution {
    func generateMatrix(_ n: Int) -> [[Int]] {
        var start: Int = 0, end: Int = n - 1
        var num: Int = 1
        var result: [[Int]] = Array(repeating: Array(repeating: 0, count: n), count: n)
        while start <= end {
            for col in start ... end {
                result[start][col] = num
                num += 1
            }

            if start < end {
                for row in start + 1 ... end {
                    result[row][end] = num
                    num += 1
                }
                for col in stride(from: end - 1, through: start, by: -1) {
                    result[end][col] = num
                    num += 1
                }
                for row in stride(from: end - 1, to: start, by: -1) {
                    result[row][start] = num
                    num += 1
                }
            }
            start += 1
            end -= 1
        }
        return result
    }
}
```