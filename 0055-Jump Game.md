
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
__O(2^nums) Time, O(1) Space - Bottom-Up Recursive, Brute-Force:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        return canJump(nums, from: 0)
    }
    
    func canJump(_ nums: [Int], from index: Int) -> Bool {
        if index >= nums.count-1 {
            return true
        }
        for i in stride(from: 1, through: nums[index], by: 1) {
            if canJump(nums, from: index+i) {
                return true
            }
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
__O(nums) Time, O(1) Space - Bottom-Up Iterative Greedy:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        
        // Use maxReachableIndex to track the highest index that can be reached
        var maxReachableIndex = 0
        
        // Iterate through nums
        // keep in mind that if maxReachableIndex is less than i, then i cannot be reached
        // from previous jumps
        for i in 0..<nums.count where i <= maxReachableIndex {
            
            // If maxReachableIndex exceeds nums.count-1, 
            // then for sure we can reach the end of nums & early return
            // In worst case i reaches nums.count-1, and we can reach the end of nums anyway
            if i == nums.count-1 || maxReachableIndex >= nums.count-1 {
                return true
            }
            
            // Keep updating maxReachableIndex along the way
            maxReachableIndex = max(i+nums[i], maxReachableIndex)
        }
        return false
    }
}
```