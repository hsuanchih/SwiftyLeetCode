
### Jump Game II

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

__Example:__
```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

__Note:__
You can assume that you can always reach the last index.

### Solution
__O(nums*min(nums, nums.max())) Time, O(nums) Space - Iterative Bottom-Up + Memoization:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {

        // If nums is empty, it takes 0 jumps to reach the end
        guard !nums.isEmpty else { return 0 }

        // memo[i] records the minimum number of jumps to reach index i
        var memo : [Int] = Array(repeating: Int.max, count: nums.count)

        // Iterate through nums
        for i in 0..<nums.count-1 {

            // Initialize memo[0] = 0
            if i == 0 {
                memo[i] = 0
            }

            // For each index reachable from i, compute the minimum jumps needs to reach such index
            for jump in stride(from: 1, through: nums[i], by: 1) where i+jump < nums.count {
                memo[i+jump] = min(memo[i]+1, memo[i+jump])
            }
        }

        // If nums has only 1 element, the loop will not enter, and memo[0] will have initial value Int.max
        // Handle this edge case
        return memo.last! < Int.max ? memo.last! : 0
    }
}
```
__O(nums) Time, O(1) Space - Iterative Bottom-Up + Greedy:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {

        // If nums is empty, it takes 0 jumps to reach the end
        guard !nums.isEmpty else { return 0 }

        // memo[i] records the minimum number of jumps to reach index i
        var memo : [Int] = Array(repeating: Int.max, count: nums.count)

        // Iterate through nums
        for i in 0..<nums.count-1 {

            // Initialize memo[0] = 0
            if i == 0 {
                memo[i] = 0
            }

            // For each index reachable from i, compute the minimum jumps needs to reach such index
            for jump in stride(from: 1, through: nums[i], by: 1) where i+jump < nums.count {
                memo[i+jump] = min(memo[i]+1, memo[i+jump])
            }
        }

        // If nums has only 1 element, the loop will not enter, and memo[0] will have initial value Int.max
        // Handle this edge case
        return memo.last! < Int.max ? memo.last! : 0
    }
}
```