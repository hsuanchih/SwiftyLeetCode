
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

__Note:__
1. You may assume that all elements in `nums` are unique.
2. `n` will be in the range `[1, 10000]`.
3. The value of each element in `nums` will be in the range `[-9999, 9999]`.

### Solution
__O(log(n)):__
```Swift
class Solution {
    func search(_ nums: [Int], _ target: Int) -> Int {
        var start = 0, end = nums.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            switch nums[mid] {
                case target:
                return mid
                case Int.min..<target:
                start = mid
                default:
                end = mid
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