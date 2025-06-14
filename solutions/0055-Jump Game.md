
### Jump Game

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

__Example 1:__
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
__Example 2:__
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

__Constraints:__
* `1 <= nums.length <= pow(10, 4)`
* `0 <= nums[i] <= pow(10, 5)`

### Solution
__O(nums!) Time, O(1) Space - Top-Down Recursive, Brute-Force:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        canReachEnd(nums, from: 0)
    }

    func canReachEnd(_ nums: [Int], from index: Int) -> Bool {
        guard index < nums.count - 1 else { return true }
        for next in stride(from: index + 1, through: index + nums[index], by: 1) where canReachEnd(nums, from: next) {
            return true
        }
        return false
    }
}

```
__O(pow(nums, 2)) Time, O(nums) Space - Top-Down Recursive + Memoization:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        var memo: [Bool?] = Array(repeating: nil, count: nums.count) 
        return canReachEnd(nums, from: 0, memo: &memo)
    }

    func canReachEnd(_ nums: [Int], from index: Int, memo: inout [Bool?]) -> Bool {
        guard index < nums.count - 1 else { return true }
        if let canReachEndFromIndex = memo[index] {
            return canReachEndFromIndex
        } else {
            for next in stride(from: index + 1, through: index + nums[index], by: 1) where canReachEnd(nums, from: next, memo: &memo) {
                memo[index] = true
                return true
            }
            memo[index] = false
            return false
        }
    }
}
```
__O(pow(nums, 2)) Time, O(nums) Space - Top-Down Iterative + Dynamic Programming:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        var dp: [Bool] = Array(repeating: false, count: nums.count)
        for start in stride(from: nums.count - 1, through: 0, by: -1) {
            if start == nums.count - 1 {
                dp[start] = true
            } else {
                for offset in stride(from: nums[start], through: 1, by: -1) where start + offset >= nums.count - 1 || dp[start + offset] {
                    dp[start] = true
                    break
                }
            }
        }
        return dp[0]
    }
}
```
__O(pow(nums, 2)) Time, O(nums) Space - Bottom-Up Iterative + Memoization:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        guard !nums.isEmpty else { return true }
        var canReach: [Bool] = Array(repeating: false, count: nums.count)
        for index in 0 ..< nums.count {
            // Initial condition, we can always reach index 0
            if index == 0 {
                canReach[index] = true
            }

            // Every other indices reachable from index is reachable if index is reachable. 
            // Update their reachability
            if canReach[index] {
                for offset in index ... index + nums[index] where offset < nums.count {
                    canReach[offset] = true
                }
            }
        }
        return canReach.last ?? false
    }
}
```
__O(nums) Time, O(1) Space - Bottom-Up Iterative Greedy:__
```swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        // Use maxReachableIndex to track the highest index that can be reached
        var maxReachableIndex: Int = 0

        // Since nums[i] is consecutive, if we can reach index i, 
        // then we can reach indices (0 ... i - 1) by deduction.
        // We can then use this property to determine the max reachable index
        for index in 0 ..< nums.count where maxReachableIndex >= index {

            // max reachable index is the global maximum of i + nums[i]
            maxReachableIndex = max(index + nums[index], maxReachableIndex)

            // If the max reachable index can reach the last index & beyond
            // we know for sure we can reach the end. Early exit.
            if maxReachableIndex >= nums.count - 1 {
                return true
            }
        }

        // If the max reachable index can reach the last index & beyond
        // we know for sure we can reach the end
        return maxReachableIndex >= nums.count - 1
    }
}
```
__O(nums) Time, O(1) Space - Bottom-Up Iterative Greedy with Slight Optimization:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        
        // Use maxReachableIndex to track the highest index that can be reached
        var maxReachableIndex: Int = 0

        // Since nums[i] is consecutive, if we can reach index i, 
        // then we can reach indices (0 ... i - 1) by deduction.
        // We can then use this property to determine the max reachable index
        for i in 0 ..< nums.count {
            // If we cannot reach i, then we cannot reach the end
            if maxReachableIndex < i {
                return false
            }
            
            // max reachable index is the global maximum of i + nums[i]
            maxReachableIndex = max(maxReachableIndex, i + nums[i])

            // If the max reachable index can reach the last index & beyond
            // we know for sure we can reach the end
            if maxReachableIndex >= nums.count - 1 {
                return true
            }
        }
        return false
    }
}
```
__O(nums) Time, O(1) Space - Top-Down Iterative Greedy:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        guard nums.count > 1 else { return true }

        // indexToReach is a moving target that tracks the index 
        // we'll need to be able to reach from the current index in order to 
        // eventually reach the last index
        var indexToReach: Int = nums.count - 1
        for index in stride(from: nums.count - 2, through: 0, by: -1) where nums[index] + index >= indexToReach {
            indexToReach = index
        }
        return indexToReach == 0
    }
}
```