
### Search in Rotated Sorted Array II

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).

You are given a target value to search. If found in the array return `true`, otherwise return `false`.

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
__Follow up:__
* This is a follow up problem to Search in Rotated Sorted Array, where `nums` may contain duplicates.
* Would this affect the run-time complexity? How and why?

### Solution
__O(n):__
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