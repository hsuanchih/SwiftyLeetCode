
### Maximum Sum Circular Subarray

Given a __circular integer array__ `nums` of length `n`, return _the maximum possible sum of a non-empty subarray_ of `nums`.

A __circular array__ means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A __subarray__ may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= k1`, `k2 <= j` with `k1 % n == k2 % n`.

__Example 1:__
```
Input: nums = [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3.
```
__Example 2:__
```
Input: nums = [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10.
```
__Example 3:__
```
Input: nums = [-3,-2,-3]
Output: -2
Explanation: Subarray [-2] has maximum sum -2.
```

__Constraints:__
* `n == nums.length`
* `1 <= n <= 3 * 10^4`
* `-3 * 10^4 <= nums[i] <= 3 * 10^4`

### Solution
__O(nums^2) Time, O(1) Space - TLE:__
```swift
class Solution {
    func maxSubarraySumCircular(_ nums: [Int]) -> Int {
        var maxSum = Int.min
        // Compute the max sum of the circular array using every index 0 ..< nums.count
        // as the starting point
        for i in 0 ..< nums.count {
            var maxSumUpToIndex = 0

            // Compute max sum using Kadane using every i as the starting point
            // and wrap around if j reaches beyond the length of the array
            for j in i ..< i+nums.count {
                let num = nums[j%nums.count]
                maxSumUpToIndex = max(maxSumUpToIndex + num, num)
                maxSum = max(maxSum, maxSumUpToIndex)
            }
        }
        return maxSum
    }
}
```

__O(nums) Time, O(1) Space:__
```swift
class Solution {
    func maxSubarraySumCircular(_ nums: [Int]) -> Int {
        // Use minSum & maxSum to track the global min & max subarray sum,
        // and minSumUpToIndex & maxSumUpToIndex to help compute minSum & maxSum using Kadane
        var minSum = Int.max
        var maxSum = Int.min
        var minSumUpToIndex = 0
        var maxSumUpToIndex = 0

        // Use totalSum to compute the total sum of the array
        var totalSum = 0
        
        for num in nums {
            maxSumUpToIndex = max(maxSumUpToIndex + num, num)
            maxSum = max(maxSum, maxSumUpToIndex)
            minSumUpToIndex = min(minSumUpToIndex + num, num)
            minSum = min(minSum, minSumUpToIndex)
            totalSum += num
        }

        // If totalSum == minSum, then the elements in the array are either all 0s or all negative values:
        // * If the elements are all 0s, maxSum will also be 0
        // * If the elements are all negative values, maxSum should be the largest single negative element in the array
        // and we want to return maxSum in this case
        // If totalSum != minSum, then:
        // * maxSum represents the global max non-cicular subarray sum (via Kadane)
        // * totalSum - minSum represents the global max subarray sum in the wrap-around circular array
        // and we want to return the maximum of the two
        return totalSum == minSum ? maxSum : max(maxSum, totalSum - minSum)
    }
}
```