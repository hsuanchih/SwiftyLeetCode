
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
__O(pow(rowIndex, 2)) Time, O(pow(rowIndex, 2)) Space:__
```Swift
class Solution {
    func getRow(_ rowIndex: Int) -> [Int] {
        var result: [[Int]] = []
        for row in 0 ... rowIndex {
            var temp: [Int] = []
            for index in 0 ... row {
                if index == 0 || index == row {
                    temp.append(1)
                } else {
                    temp.append(result[row - 1][index - 1] + result[row - 1][index])
                }
            }
            result.append(temp)
        }
        return result.last ?? [] 
    }
}
```
