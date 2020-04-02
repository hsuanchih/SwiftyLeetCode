
### Largest Rectangle in Histogram

Given __n__ non-negative integers representing the histogram's bar height where the width of each bar is `1`,</br> 
find the area of largest rectangle in the histogram.

![Histogram](images/question_84-0.png)

Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

![Example](images/question_84-1.png)

The largest rectangle is shown in the shaded area, which has area = 10 unit.

__Example:__
```
Input: [2,1,5,6,2,3]
Output: 10
```

### Solution
__O(heights^2) Time, O(1) Space:__
```Swift
class Solution {
    func largestRectangleArea(_ heights: [Int]) -> Int {
        var result = 0

        // For each index i, compute the maximum area to its left
        for i in 0..<heights.count {
            var min = heights[i]
            for j in stride(from: i, through: 0, by: -1) {
                min = Swift.min(heights[j], min)
                result = max(result, min*(i-j+1))
            }
        }
        return result
    }
}
```
__O(heights^2) Time, O(1) Space - Optimized:__
```Swift
class Solution {
    func largestRectangleArea(_ heights: [Int]) -> Int {
        var result = 0

        // For each index i, compute the maximum area to its left only if the next bar
        // is shorter than the current
        // If the next bar is taller, we're guaranteed that the max area at index i will
        // not be the global max
        for i in 0..<heights.count {
            var min = heights[i]
            if i == heights.count-1 || (i < heights.count-1 && heights[i+1] < heights[i]) {
                for j in stride(from: i, through: 0, by: -1) {
                    min = Swift.min(heights[j], min)
                    result = max(result, min*(i-j+1))
                }
            }
        }
        return result
    }
}
```
__O(heights) Time, O(heights) Space - Stack:__
```Swift
class Solution {
    func largestRectangleArea(_ heights: [Int]) -> Int {
        var stack : [Int] = [], maxArea = 0, i = 0
        while i < heights.count {
            guard let last = stack.last, heights[i] < heights[last] else {
                stack.append(i)
                i+=1
                continue
            }
            let curr = stack.removeLast()
            if let top = stack.last {
                maxArea = max(maxArea, heights[curr]*(i-top-1))
            } else {
                maxArea = max(maxArea, heights[curr]*i)
            }
        }
        while !stack.isEmpty {
            let curr = stack.removeLast()
            if let top = stack.last {
                maxArea = max(maxArea, heights[curr]*(i-top-1))
            } else {
                maxArea = max(maxArea, heights[curr]*i)
            }
        }
        return maxArea
    }
}
```
