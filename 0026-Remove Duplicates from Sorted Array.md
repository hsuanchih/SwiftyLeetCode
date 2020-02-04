
### Remove Duplicates from Sorted Array

Given a sorted array nums, remove the duplicates __in-place__ such that each element appear only once and return the new length.</br>
Do not allocate extra space for another array, you must do this by __modifying the input array__ in-place with O(1) extra memory.

__Example 1:__
```
Given nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the returned length.
```
__Example 2:__
```
Given nums = [0,0,1,1,1,2,2,3,3,4],
Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.
It doesn't matter what values are set beyond the returned length.
```

### Solution
__O(n):__
```Swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        if nums.isEmpty {
            return 0
        }

        // Everything before end is unique
        var end = 0

        // Iterate through the array and swap element into end if the 2 values are different
        for i in 0..<nums.count {
            if nums[i] != nums[end] {
                end+=1
                nums.swapAt(i, end)
            }
        }
        return end+1
    }
}
```