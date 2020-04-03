
### Largest Rectangle in Histogram

Given __n__ non-negative integers representing the histogram's bar height where the width of each bar is `1`,</br> 
find the area of largest rectangle in the histogram.


Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].


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
        
        // Use stack to store the indices of a monotonic increasing bar height
        var stack: [Int] = [], result = 0
        for i in 0..<heights.count {
            
            // We want to continue popping from the stack as long as:
            // 1. The stack is not empty, and
            // 2. The height of the bar at the end of the stack is taller than the bar at index i
            // than the last bar on the stack
            while let last = stack.last, heights[last] > heights[i] {
                let index = stack.removeLast(), height = heights[index]
                
                // There are 2 scenarios of computing the area under the bar at index
                // 1. There is a bar to the left of index on the stack, ie, stack not empty
                // width = i-left-1, area = height*width
                // 2. There are no bars left on the stack, ie, stack is empty
                // width = i-0, area = height*i
                if let left = stack.last {
                    result = max(result, height*(i-left-1))
                } else {
                    result = max(result, height*i)
                }
            }
            
            // We are ready to push the bar at the current index onto the stack, now
            // that there are no bars on the stack that are taller than the bar at i
            stack.append(i)
        }
        
        // Empty the indices from the stack while computing the area for each bar on the stack
        while !stack.isEmpty {
            let index = stack.removeLast(), height = heights[index]
            
            // We simulate a hypothetical bar of height 0 at index heights.count
            // to compute the area of each remaining bar that's still on the stack
            if let left = stack.last {
                result = max(result, height*(heights.count-left-1))
            } else {
                result = max(result, height*(heights.count))
            }
        }
        return result
    }
}
```