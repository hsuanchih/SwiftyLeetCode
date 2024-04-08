
### Pascal's Triangle

Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

![In Pascal's triangle, each number is the sum of the two numbers directly above it.](../images/question_118.gif)


__Example 1:__
```
Input: numRows = 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
__Example 2:__
```
Input: numRows = 1
Output: [[1]]
```

### Solution
__O(pow(numRows, 2)) Time, O(pow(numRows, 2)) Space:__
```Swift
class Solution {
    func generate(_ numRows: Int) -> [[Int]] {
        var result: [[Int]] = []
        for row in 0 ..< numRows {
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
        return result
    }
}
```
