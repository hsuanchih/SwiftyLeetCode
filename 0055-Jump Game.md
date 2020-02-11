
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
__O(n^2):__
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
__O(n) Time, O(n) Space:__
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
__O(n) Time, O(1) Space:__
```Swift
class Solution {
    func canJump(_ nums: [Int]) -> Bool {
        let numsCount = nums.count
        var lastReachable = numsCount-1
        for i in stride(from: numsCount-2, through: 0, by: -1) {
            if i + nums[i] >= lastReachable {
                lastReachable = i
            }
        }
        return lastReachable == 0
    }
}
```