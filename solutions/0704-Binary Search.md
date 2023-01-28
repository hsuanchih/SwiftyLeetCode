
### Binary Search

Given a __sorted__ (in ascending order) integer array `nums` of `n` elements and a target value,</br> 
write a function to search `target` in `nums`. If `target` exists, then return its index, otherwise return `-1`.

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
* `1 <= nums.length <= 10^4`
* `-10^4 < nums[i], target < 10^4`
* All the integers in `nums` are unique.
* `nums` is sorted in ascending order.

### Solution
__O(log\[base 2\](nums)) Time:__
```Swift
class Solution {
    func search(_ nums: [Int], _ target: Int) -> Int {
        var start: Int = 0, end: Int = nums.count - 1
        while start + 1 < end {
            let mid = start + (end - start) / 2
            switch nums[mid] {
            case target:
                return mid
            case Int.min ..< target:
                start = mid
            case target + 1 ... Int.max:
                end = mid
            default:
                fatalError()
            }
        }
        switch (nums[start], nums[end]) {
        case (target, _):
            return start
        case (_, target):
            return end
        default:
            return -1
        }
    }
}
```