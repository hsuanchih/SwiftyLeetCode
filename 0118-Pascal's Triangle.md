
### Pascal's Triangle

Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it.

__Example:__
```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

### Solution
```Swift
class Solution {
    func generate(_ numRows: Int) -> [[Int]] {
        if numRows == 0 {
            return []
        }
        var result : [[Int]] = [[1]]
        for row in 1..<numRows {
            let prev = result[row-1]
            var temp = Array(repeating: 1, count: row+1)
            for col in 0...row {
                switch col {
                    case 0, row:
                    temp[col] = 1
                    default:
                    temp[col] = prev[col-1] + prev[col]
                }
            }
            result.append(temp)
        }
        return result
    }
}
```