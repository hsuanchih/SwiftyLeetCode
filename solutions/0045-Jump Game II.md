
### Jump Game II

You are given a `0`-indexed array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index i. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:
* `0 <= j <= nums[i]` and
* `i + j < n`

Return the minimum number of jumps to reach `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

__Example 1:__
```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
__Example 2:__
```
Input: nums = [2,3,0,1,4]
Output: 2
```
__Example 3:__
```
Input: nums = [7,0,9,6,9,6,1,7,9,0,1,2,9,0,3]
Output: 2
```

__Constraints:__
* `1 <= nums.length <= pow(10, 4)`
* `0 <= nums[i] <= 1000`
* It's guaranteed that you can reach `nums[n - 1]`.

### Solution
__O(pow(nums, nums)) Time, O(1) Space - Recursive Bottom-Up Brute Force:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {
        minJumps(nums, from: 0)
    }

    func minJumps(_ nums: [Int], from index: Int) -> Int {
        // Base case: 0 jumps to reach the end of the array if we're already at the end
        guard index < nums.count - 1 else { return 0 }

        // Recursive definition:
        // The minimum jumps required to reach the end from index is the minimum jumps
        // from each of the indices reachable from index + 1
        var jumps: Int = Int.max
        for offset in stride(from: 1, through: nums[index], by: 1) {
            jumps = min(jumps, minJumps(nums, from: index + offset))
        }
        return jumps == Int.max ? jumps : 1 + jumps
    }
}
```
__O(pow(nums, 2)) Time, O(nums) Space - Recursive Bottom-Up + Memoization:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {
        var memo: [Int?] = Array(repeating: nil, count: nums.count)
        return minJumps(nums, from: 0, memo: &memo)
    }

    func minJumps(_ nums: [Int], from index: Int, memo: inout [Int?]) -> Int {
        guard index < nums.count - 1 else { return 0 }
        if let jumps = memo[index] {
            return jumps
        }
        var jumps: Int = Int.max
        for offset in stride(from: 1, through: nums[index], by: 1) {
            jumps = min(jumps, minJumps(nums, from: index + offset, memo: &memo))
        }
        memo[index] = jumps == Int.max ? jumps : 1 + jumps
        return memo[index]!
    }
}
```
__O(pow(nums, 2))) Time, O(nums) Space - Iterative Bottom-Up + Memoization:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {
        // minStepsToIndex[i] records the minimum number of steps to reach index i
        var minStepsToIndex: [Int] = Array(repeating: .max, count: nums.count)

        for index in 0 ..< nums.count {
            if index == 0 {
                minStepsToIndex[index] = 0
            }

            // For each index reachable from i, compute the minimum jumps needs to reach such index
            for offset in stride(from: 1, through: nums[index], by: 1) where index + offset < nums.count {
                minStepsToIndex[index + offset] = min(minStepsToIndex[index + offset], minStepsToIndex[index] + 1)
            }
        }
        return minStepsToIndex.last ?? 0
    }
}
```
__O(nums) Time, O(1) Space - Iterative Bottom-Up + Greedy:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {
        guard nums.count > 1 else { return 0 }
        var k: Int = 0

        // maxIndexReachableInKSteps tracks the max index reachable in k steps
        var maxIndexReachableInKSteps: Int = 0

        // maxIndexReachable tracks the max index reachable at any time
        var maxIndexReachable: Int = 0

        for index in 0 ..< nums.count {

            // If we've reached the max index reachable in k steps,
            // increment k and update the max index reachable in k+1 steps with the global max
            if maxIndexReachableInKSteps < index {
                k += 1
                maxIndexReachableInKSteps = maxIndexReachable
            }

            // Update the global max reachable index unconditionally
            maxIndexReachable = max(maxIndexReachable, index + nums[index])

            // We can reach the end from the current index
            // return k + 1
            if maxIndexReachable >= nums.count - 1 {
                return k + 1
            }
        }

        // There is no solution
        fatalError()
    }
}
```