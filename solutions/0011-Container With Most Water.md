
### Container With Most Water

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

__Example 1:__

![images/question_11.jpg](../images/question_11.jpg)
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```
__Example 2:__
```
Input: height = [1,1]
Output: 1
```

__Constraints:__
* `n == height.length`
* `2 <= n <= pow(10, 5)`
* `0 <= height[i] <= pow(10, 4)`

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
