
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
__O(pow(nums, 3)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        // Initial maxSum need to be Int.min since an element can be a negative value
        var maxSum: Int = .min
        for start in 0 ..< nums.count {
            for end in start ..< nums.count {
                maxSum = max(maxSum, sumOfSubarray(nums, start ... end))
            }
        }
        return maxSum
    }

    func sumOfSubarray(_ nums: [Int], _ range: ClosedRange<Int>) -> Int {
        range.reduce(into: 0) { result, index in
            result += nums[index]
        }
    }
}
```
__O(pow(nums, 2)) Time, O(1) Space - Improved Brute-Force:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        // Initial maxSum need to be Int.min since an element can be a negative value
        var maxSum: Int = .min
        for start in 0 ..< nums.count {
            var sum: Int = 0
            for end in start ..< nums.count {
                sum += nums[end]
                maxSum = max(maxSum, sum)
            }
        }
        return maxSum
    }
}
```
__O(nums) Time, O(nums) Space - Memoization:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        guard !nums.isEmpty else { return 0 }
        var maxAtIndex: [Int] = Array(repeating: nums.first!, count: nums.count)

        // The initial value of maxSum needs to be the first element to account for the
        // case where nums only includes 1 element
        var maxSum: Int = nums.first!
        for index in 1 ..< nums.count {
            maxAtIndex[index] = max(nums[index], maxAtIndex[index - 1] + nums[index])
            maxSum = max(maxSum, maxAtIndex[index])
        }
        return maxSum
    }
}
```
__O(nums) Time, O(1) Space - Improved Memoization:__
```Swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var maxSum: Int = .min
        var prefixSum: Int = 0
        nums.forEach { num in
            let sum = prefixSum + num
            maxSum = max(maxSum, sum)

            // If the prefixSum including the current element is less than 0,
            // we want to reset prefixSum to 0 as we no longer want to cumulate 
            // the prefixSum for the next element
            prefixSum = max(0, sum)
        }
        return maxSum
    }
}
```