
### Trapping Rain Water

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

__Example 1:__

![question_42.png](../images/question_42.png)
```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```
__Example 2:__
```
Input: height = [4,2,0,3,2,5]
Output: 9
```

__Constraints:__
* `n == height.length`
* `1 <= n <= 2 * pow(10, 4)`
* `0 <= height[i] <= pow(10, 5)`

### Solution
__O(pow(height, 2)) Time, O(1) Space - Brute Force:__
```Swift
class Solution {
    func trap(_ height: [Int]) -> Int {
        var result: Int = 0
        
        // Iterate through each element of the input
        // Ignore indices 0 & input.count-1 as they cannot trap any water
        for i in 0 ..< height.count where i > 0 && i < height.count - 1 {
            
            // The water that can trapped at height[i] is:
            // min(tallest bar to the left of i, tallest bar to the right of i) - height[i]
            // This computation is O(height)
            let heightOfWater = min(height[0 ..< i].max() ?? 0, height[(i + 1) ..< height.count].max() ?? 0)
            
            // If we cannot find a pair of bars to the left & right of height[i] that are both taller than
            // height[i], then the amount of water height[i] can trap is 0
            result += max(heightOfWater - height[i], 0)
        }
        return result
    }
}
```
__(pow(height, 2) / 2) Time, (1) Space - Prefix Maximum:__
```Swift
class Solution {
    func trap(_ height: [Int]) -> Int {
        
        // Use maxHeightSoFar to track the maximum bar height to the left of height[i]
        var maxHeightSoFar = 0
        var result = 0
        
        // Iterate through each element of the input
        for i in 0..<height.count {
            
            // Update maxHeightSoFar
            defer { maxHeightSoFar = max(maxHeightSoFar, height[i]) }
            
            // Ignore indices 0 & input.count-1 as they cannot trap any water
            guard i > 0 && i < height.count-1 else { continue }
            
            // The water that can trapped at height[i] is:
            // min(tallest bar to the left of i, tallest bar to the right of i) - height[i]
            let heightOfWater = min(maxHeightSoFar, height[i+1..<height.count].max() ?? 0)
            
            // If we cannot find a pair of bars to the left & right of height[i] that are both taller than
            // height[i], then the amount of water height[i] can trap is 0
            result += max(heightOfWater-height[i], 0)
        }
        return result
    }
}
```
__O(height*2) Time, O(height) Space - Prefix Maximum + Memoized Postfix Maximum:__
```Swift
class Solution {
    func trap(_ height: [Int]) -> Int {
        
        // Use maxHeightSoFar to track the maximum bar height to the left of height[i]
        // Use maxHeightAfterIndex to track the maximum bar height to the right of index i
        var maxHeightSoFar = 0, maxHeightAfterIndex: [Int] = Array(repeating: 0, count: height.count)
        var result = 0
        
        // Compute maxHeightAfterIndex for every index i
        for i in stride(from: height.count, through: 0, by: -1) {
            
            // The max height after index height.count-1 should be 0 since
            // it is the last element of the input
            if i >= height.count-1 {
                continue
            }
            maxHeightAfterIndex[i] = max(height[i+1], maxHeightAfterIndex[i+1])
        }
        
        // Iterate through each element of the input
        for i in 0..<height.count {
            
            // Update maxHeightSoFar
            defer { maxHeightSoFar = max(maxHeightSoFar, height[i]) }
            
            // Ignore indices 0 & input.count-1 as they cannot trap any water
            guard i > 0 && i < height.count-1 else { continue }
            
            // The water that can trapped at height[i] is:
            // min(tallest bar to the left of i, tallest bar to the right of i) - height[i]
            let heightOfWater = min(maxHeightSoFar, maxHeightAfterIndex[i])
            
            // If we cannot find a pair of bars to the left & right of height[i] that are both taller than
            // height[i], then the amount of water height[i] can trap is 0
            result += max(heightOfWater-height[i], 0)
        }
        return result
    }
}
```
__O(height) Time, O(1) Space - Two Pointer:__
```Swift
class Solution {
    func trap(_ height: [Int]) -> Int {
        guard !height.isEmpty else { return 0 }
        
        // Use left & right indices to track the left & right pointers
        var left = 0, right = height.count-1
        
        // Use maxHeightLeft & maxHeightRight to track the max bar heights up to 
        // indices left & right from both ends
        var maxHeightLeft = height[left], maxHeightRight = height[right]
        
        var result = 0
        
        // We're interested the minimum of maxHeightLeft & maxHeightRight,
        // as the current height[i] only depends on the smaller of the two
        while left < right {
            if maxHeightLeft < maxHeightRight {
                result += maxHeightLeft-height[left]
                maxHeightLeft = max(maxHeightLeft, height[left+1])
                left+=1
            } else {
                result += maxHeightRight-height[right]
                maxHeightRight = max(maxHeightRight, height[right-1])
                right-=1
            }
        }
        return result
    }
}
```