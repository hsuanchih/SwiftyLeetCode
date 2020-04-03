
### Largest Rectangle in Histogram

Given a 2D binary matrix filled with `0`'s and `1'`s, find the largest rectangle containing only `1`'s and return its area.

__Example:__
```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

### Solution
__O((matrix.row\*matrix.col)\*2) Time, O(matrix.row\*matrix.col+matrix.col) Space - Memoization + Stack:__
```Swift
class Solution {
    func maximalRectangle(_ matrix: [[Character]]) -> Int {
        
        // Convert matrix into an array of histograms
        let histogram = self.histogram(matrix)
        
        // Compute the max area of each histogram, 
        // and the maximum area from all max areas
        var result = 0
        for row in 0..<histogram.count {
            result = max(result, maxAreaHistogram(histogram[row]))
        }
        return result
    }
    
    // Helper method to convert each row of matrix into a histogram
    func histogram(_ matrix: [[Character]]) -> [[Int]] {
        var result : [[Int]] = Array(repeating: Array(repeating: 0, count: matrix.first?.count ?? 0), count: matrix.count)
        for row in 0..<matrix.count {
            for col in 0..<(matrix.first?.count ?? 0) {
                if row == 0 {
                    result[row][col] = matrix[row][col] == "1" ? 1 : 0
                } else {
                    result[row][col] = matrix[row][col] == "0" ? 0 : result[row-1][col]+1
                }
            }
        }
        return result
    }
    
    // Compute the max area given a histogram of heights
    // See Leetcode 84 - Largest Rectangle in Histogram
    func maxAreaHistogram(_ heights: [Int]) -> Int {
        var stack : [Int] = [], result = 0
        for i in 0..<heights.count {
            while let last = stack.last, heights[last] > heights[i] {
                let index = stack.removeLast(), height = heights[index]
                if let left = stack.last {
                    result = max(result, height*(i-left-1))
                } else {
                    result = max(result, height*i)
                }
            }
            stack.append(i)
        }
        
        while !stack.isEmpty {
            let index = stack.removeLast(), height = heights[index]
            if let left = stack.last {
                result = max(result, height*(heights.count-left-1))
            } else {
                result = max(result, height*heights.count)
            }
        }
        return result
    }
}
```