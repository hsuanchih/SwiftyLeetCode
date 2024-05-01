
### Rotate Array

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

__Example 1:__
```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```
__Example 2:__
```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

__Constraints:__
* `1 <= nums.length <= pow(10, 5)`
* `-pow(2, 31) <= nums[i] <= pow(2, 31) - 1`
* `0 <= k <= pow(10, 5)`
 
__Follow up:__
* Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.
* Could you do it in-place with `O(1)` extra space?

### Solution
__O(nums) Time:__
```Swift
class Solution {
    func rotate(_ nums: inout [Int], _ k: Int) {
        let k: Int = k % nums.count
        guard k != 0 else { return }
        reverse(&nums, 0 ... (nums.count - k - 1))
        reverse(&nums, nums.count - k ... nums.count - 1)
        reverse(&nums, 0 ... nums.count - 1)
    }

    func reverse(_ nums: inout [Int], _ range: ClosedRange<Int>) {
        var start: Int = range.lowerBound, end: Int = range.upperBound
        while start < end {
            nums.swapAt(start, end)
            start += 1
            end -= 1
        }
    }
}
```