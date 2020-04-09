
### Maximum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

__Example:__
```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```
__Follow up:__

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

### Solution
__O(nums^3) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var result = Int.min
        for start in stride(from: 0, to: nums.count, by: 1) {
            for end in stride(from: start, to: nums.count, by: 1) {
                var sum = 0
                for index in stride(from: start, through: end, by: 1) {
                    sum+=nums[index]
                }
                result = max(result, sum)
            }
        }
        return result
    }
}
```
__O(nums^2) Time, O(1) Space - Improved Brute-Force:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var result = Int.min
        for i in stride(from: 0, to: nums.count, by: 1) {
            var sum = 0
            for j in i..<nums.count {
                sum+=nums[j]
                result = max(result, sum)
            }
        }
        return result
    }
}
```
__O(nums) Time, O(nums) Space - Memoization:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var maxAtIndex : [Int] = Array(repeating: nums.first!, count: nums.count), result : Int = nums.first!
        for i in stride(from: 1, to: nums.count, by: 1) {
            maxAtIndex[i] = max(nums[i], maxAtIndex[i-1]+nums[i])
            result = max(result, maxAtIndex[i])
        }
        return result
    }
}
```
__O(nums) Time, O(1) Space - Improved Memoization:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var currMax = nums.first!, result = currMax
        for i in stride(from: 1, to: nums.count, by: 1) {
            currMax = max(nums[i], currMax+nums[i])
            result = max(result, currMax)
        }
        return result
    }
}
```