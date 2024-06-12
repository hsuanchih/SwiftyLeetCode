
### Minimum Size Subarray Sum

Given an array of positive integers `nums` and a positive integer `target`, return the __minimal length__ of a 
subarray whose sum is __greater than or equal to__ `target`. If there is no such subarray, return `0` instead.

__Example 1:__
```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```
__Example 2:__
```
Input: target = 4, nums = [1,4,4]
Output: 1
```
__Example 3:__
```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

__Constraints:__
* `1 <= target <= pow(10, 9)`
* `1 <= nums.length <= pow(10, 5)`
* `1 <= nums[i] <= pow(10, 4)`

__Follow up:__
* If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`. 

### Solution
__O(pow(nums, 2)) Time - Brute-Force:__
```Swift
class Solution {
    func minSubArrayLen(_ target: Int, _ nums: [Int]) -> Int {
        var minSize: Int = .max
        for start in 0 ..< nums.count {
            var sum: Int = 0
            for end in start ..< nums.count {
                sum += nums[end]
                if sum >= target {
                    minSize = min(minSize, end - start + 1)
                    break
                }
            }
        }
        return minSize == .max ? 0 : minSize
    }
}
```
__O(nums * log(nums)) Time - Prefix Sum + Binary Search:__
```Swift
class Solution {
    func minSubArrayLen(_ target: Int, _ nums: [Int]) -> Int {
        var prefixSum: [Int] = Array(repeating: 0, count: nums.count)
        var sum: Int = 0
        var minSize: Int = .max
        for i in 0 ..< nums.count {
            sum += nums[i]
            prefixSum[i] = sum
            if sum >= target {
                minSize = min(minSize, i + 1)

                // Binary search on prefix sum
                var start: Int = 0
                var end: Int = i
                while start + 1 < end {
                    let mid: Int = start + (end - start) / 2
                    switch sum - prefixSum[mid] {
                    case target...:
                        start = mid
                    case ..<target:
                        end = mid
                    default:
                        fatalError()
                    }
                }
                if sum - prefixSum[end] >= target {
                    minSize = min(minSize, i - end)
                } else if sum - prefixSum[start] >= target {
                    minSize = min(minSize, i - start)
                }
            }
        }
        return minSize == .max ? 0 : minSize
    }
}
```
__O(nums) Time - Sliding-Window:__
```Swift
class Solution {
    func minSubArrayLen(_ target: Int, _ nums: [Int]) -> Int {
        var start: Int = 0
        var sum: Int = 0
        var minSize: Int = .max
        for end in 0 ..< nums.count {
            sum += nums[end]
            while sum >= target {
                minSize = min(minSize, end - start + 1)
                sum -= nums[start]
                start += 1
            }
        }
        return minSize == .max ? 0 : minSize
    }
}
```