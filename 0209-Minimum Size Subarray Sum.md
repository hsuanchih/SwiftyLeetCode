
### Minimum Size Subarray Sum

Given an array of __n__ positive integers and a positive integer __s__,</br>
find the minimal length of a contiguous subarray of which the sum ≥ __s__.
If there isn't one, return 0 instead.

__Example:__
```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```
__Follow up:__
If you have figured out the O(*n*) solution, try coding another solution of which the time complexity is O(*n* log *n*). 

### Solution
__O(n^2):__
```Swift
class Solution {
    func minSubArrayLen(_ s: Int, _ nums: [Int]) -> Int {
        var result = Int.max
        for start in stride(from: 0, to: nums.count, by: 1) {
            var sum = 0
            for end in stride(from: start, to: nums.count, by: 1) {
                sum+=nums[end]
                if sum >= s {
                    result = min(result, end-start+1)
                }
            }
        }
        return result == Int.max ? 0 : result
    }
}
```
__O(2*n):__
```Swift
class Solution {
    func minSubArrayLen(_ s: Int, _ nums: [Int]) -> Int {
        var sum = 0, start = 0, result = Int.max
        for end in stride(from: 0, to: nums.count, by: 1) {
            sum += nums[end]
            while sum >= s {
                result = min(result, end-start+1)
                sum -= nums[start]
                start+=1
            }
        }
        return result == Int.max ? 0 : result
    }
}
```