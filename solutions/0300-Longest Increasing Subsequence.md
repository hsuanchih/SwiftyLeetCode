
### Longest Increasing Subsequence

Given an unsorted array of integers, find the length of longest increasing subsequence.

__Example:__
```
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
```

__Note:__

* There may be more than one LIS combination, it is only necessary for you to return the length.
* Your algorithm should run in O(n2) complexity.

__Follow up:__ Could you improve it to O(n log n) time complexity?

### Solution
__O(2^nums) Time, O(1) Space - Bottom-Up Recursive:__
```Swift
class Solution {
    func lengthOfLIS(_ nums: [Int]) -> Int {
        return longest(nums, Int.min, 0)
    }
    
    func longest(_ nums: [Int], _ prev: Int, _ index: Int) -> Int {
        if index == nums.count {
            return 0
        }

        // There are 2 options we can explore here:
        // 1. Ignore the current element; continue finding LIS from the next index
        // 2. If the current element is greater than the previous, we can include
        //    the current element as part of our LIS
        var max = longest(nums, prev, index+1)
        if nums[index] > prev {
            max = Swift.max(max, longest(nums, nums[index], index+1)+1)
        }
        return max
    }
}
```
__O(nums^2) Time, O(nums) Space:__
```Swift
class Solution {
    func lengthOfLIS(_ nums: [Int]) -> Int {
        var memo : [Int] = Array(repeating: 1, count: nums.count), result = 1
        for end in stride(from: 1, to: nums.count, by: 1) {
            for start in stride(from: 0, through: end, by: 1) {
                if nums[end] > nums[start] {
                    memo[end] = max(memo[end], memo[start]+1)
                }
                result = max(result, memo[end])
            }
        }
        return memo.isEmpty ? 0 : result
    }
}
```