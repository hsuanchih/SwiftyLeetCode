
### Pascal's Triangle II

Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle.

Note that the row index starts from 0.

![In Pascal's triangle, each number is the sum of the two numbers directly above it.](./images/question_118.gif)

__Example:__
```
Input: 3
Output: [1,3,3,1]
```
__Follow up:__
Could you optimize your algorithm to use only O(k) extra space?

### Solution
```Swift
class Solution {
    func getRow(_ rowIndex: Int) -> [Int] {
        var rowSum = Array(repeating: 1, count: rowIndex+1)
        if rowIndex < 2 {
            return rowSum
        }
        for row in 2...rowIndex {
            for col in stride(from: row-1, through: 1, by: -1) {
                rowSum[col] = rowSum[col-1] + rowSum[col]
            }
        }
        return rowSum
    }
}
```
