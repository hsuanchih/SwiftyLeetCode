
### Search in Rotated Sorted Array II

There is an integer array `nums` sorted in non-decreasing order (not necessarily with distinct values).

Before being passed to your function, `nums` is rotated at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (__0-indexed__). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` after the rotation and an integer `target`, return `true` if target is in `nums`, or `false` if it is not in `nums`.

You must decrease the overall operation steps as much as possible.

__Example 1:__
```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```
__Example 2:__
```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

__Constraints:__
* `1 <= nums.length <= 5000`
* `-pow(10, 4) <= nums[i] <= pow(10, 4)`
* `nums` is guaranteed to be rotated at some pivot.
* `-pow(10, 4) <= target <= pow(10, 4)`

__Follow up:__
* This is a follow up problem to Search in Rotated Sorted Array, where `nums` may contain duplicates.
* Would this affect the run-time complexity? How and why?

### Solution
__O(nums) Time:__
```Swift
class Solution {
    func search(_ nums: [Int], _ target: Int) -> Bool {
        for num in nums {
            if num == target {
                return true
            }
        }
        return false
    }
}
```