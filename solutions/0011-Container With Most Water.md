
### Container With Most Water

Given n non-negative integers *a1, a2, ..., an* , where each represents a point at coordinate *(i, ai)*.</br> 
*n* vertical lines are drawn such that the two endpoints of line i is at *(i, ai)* and *(i, 0)*.</br> 
Find two lines, which together with x-axis forms a container, such that the container contains the most water.

__Note:__ You may not slant the container and n is at least 2.

![images/question_11.jpg](images/question_11.jpg)

__Example:__
```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

### Solution
__O(pow(height, 2)) Time, O(1) Space - Maximize Container Capacity at Each Start/End Pair:__
```Swift
class Solution {
    func maxArea(_ height: [Int]) -> Int {
        var result: Int = 0

        // Validate the area between each pair of start/end points
        for start in 0 ..< height.count {
            for end in start ..< height.count {
                result = max(result, min(height[start], height[end]) * (end - start))
            }
        }
        return result
    }
}
```
__O(height) Time, O(1) Space - 2 Pointers:__
```Swift
class Solution {
    func maxArea(_ height: [Int]) -> Int {
        var result: Int = 0
        var start: Int = 0, end: Int = height.count - 1

        // Move in from both ends of the array, retain the taller of the two endpoints
        // as we advance the next index
        while start < end {
            result = max(result, min(height[start], height[end]) * (end - start))
            if height[start] < height[end] {
                start += 1
            } else {
                end -= 1
            }
        }
        return result
    }
}
```
