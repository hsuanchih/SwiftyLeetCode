
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
__O(nums^2)) Time, O(nums) Space - Iterative Bottom-Up + Memoization:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {
        // If nums is empty, it takes 0 jumps to reach the end
        guard !nums.isEmpty else { return 0 }

        // minStepsRequiredToReachIndex[i] records the minimum number of steps to reach index i
        var minStepsRequiredToReachIndex: [Int] = Array(repeating: Int.max, count: nums.count)
        minStepsRequiredToReachIndex[0] = 0

        // Iterate through nums
        for index in 0 ..< nums.count - 1 {

            // For each index reachable from i, compute the minimum jumps needs to reach such index
            for next in 0 ..< nums[index] where index + next + 1 < nums.count {
                minStepsRequiredToReachIndex[index + next + 1] = min(minStepsRequiredToReachIndex[index] + 1, minStepsRequiredToReachIndex[index + next + 1])
            }
        }
        return minStepsRequiredToReachIndex.last ?? 0
    }
}
```
__O(nums) Time, O(1) Space - Iterative Bottom-Up + Greedy:__
```Swift
class Solution {
    func jump(_ nums: [Int]) -> Int {
        // If nums is of size 1 or less, it will take 0 steps to reach the end
        if nums.count <= 1 { return 0 }

        // It takes at least 1 step to reach the end of nums
        var k: Int = 1

        // maxIndexReachableInKSteps tracks the max index reachable in k steps
        var maxIndexReachableInKSteps: Int = nums.first!

        // maxIndexReachable tracks the max index reachable at any time
        var maxIndexReachable: Int = maxIndexReachableInKSteps

        // If we can reach the end of nums from the 0th index, then the number of
        // steps required is just 1
        guard maxIndexReachable < nums.count - 1 else { return k }

        // Iterate through nums
        for index in 1 ..< nums.count {

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
            if maxIndexReachable >= nums.count-1 {
                return k + 1
            }
        }

        // There is no solution, return -1.
        return -1
    }
}
```