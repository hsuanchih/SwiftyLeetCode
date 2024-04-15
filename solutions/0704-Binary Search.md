
### Binary Search

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

__Example 1:__
```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```
__Example 2:__
```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

__Constraints:__
* `1 <= nums.length <= pow(10, 4)`
* `-pow(10, 4) < nums[i], target < pow(10, 4)`
* All the integers in `nums` are unique.
* `nums` is sorted in ascending order.

### Solution
__O(log(nums)) Time:__
```Swift
class Solution {
    func search(_ nums: [Int], _ target: Int) -> Int {
        var start: Int = 0, end: Int = nums.count - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            switch nums[mid] - target {
            case 0:
                return mid
            case ..<0:
                start = mid
            case 1...:
                end = mid
            default:
                fatalError()
            }
        }
        if nums[start] == target {
            return start
        } else if nums[end] == target {
            return end
        } else {
            return -1
        }
    }
}
```