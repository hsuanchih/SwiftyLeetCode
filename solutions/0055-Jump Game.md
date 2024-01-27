
### Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

__Example 1:__
```
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
__Example 2:__
```
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

### Solution
__O(pow(nums, nums)) Time, O(1) Space - Bottom-Up Recursive, Brute-Force:__
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
__O(nums^2) Time, O(nums) Space - Bottom-Up Recursive + Memoization:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        var memo : [Bool?] = Array(repeating: nil, count: nums.count)
        return canJump(nums, from: 0, &memo)
    }
    
    func canJump(_ nums: [Int], from index: Int, _ memo: inout [Bool?]) -> Bool {
        if index >= nums.count-1 {
            return true
        }
        if let result = memo[index] {
            return result
        }
        for i in stride(from: 1, through: nums[index], by: 1) {
            if canJump(nums, from: index+i, &memo) {
                memo[index] = true
                return true
            }
        }
        memo[index] = false
        return false
    }
}
```
__O(nums^2) Time, O(nums) Space - Bottom-Up Iterative + Memoization:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        var canReach: [Bool] = Array(repeating: false, count: nums.count)
        for i in 0 ..< nums.count {
            // Initial condition, we can always reach index 0
            if i == 0 {
                canReach[i] = true
            }

            // For every other indices reachable from i, update their reachability
            for next in (0 ... nums[i]) where i + next < canReach.count {
                // An future index can be reached if the current index is reachable
                canReach[i + next] = canReach[i + next] || canReach[i]
            }
        }
        return canReach.last ?? false
    }
}
```
__O(nums) Time, O(1) Space - Bottom-Up Iterative Greedy:__
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