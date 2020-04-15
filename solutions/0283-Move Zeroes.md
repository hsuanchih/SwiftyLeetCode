
### Find the Duplicate Number

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order</br> 
of the non-zero elements.

__Example:__
```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

__Note:__
1. You must do this __in-place__ without making a copy of the array.
2. Minimize the total number of operations.

### Solution
__O(nums) Time, O(1) Space:__
```Swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        var left = 0
        for index in 0..<nums.count {
            if nums[index] != 0 {
                if index != left {
                    nums.swapAt(left, index)
                }
                left+=1
            }
        }
    }
}
```