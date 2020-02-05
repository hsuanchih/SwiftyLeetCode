
### Remove Element

Given an array nums and a value val, remove all instances of that value in-place and return the new length.</br>
Do not allocate extra space for another array, you must do this by __modifying the input array in-place__ with O(1) extra memory.</br>
The order of elements can be changed. It doesn't matter what you leave beyond the new length.

__Example 1:__
```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```
__Example 2__:
```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```
### Solution
__O(n):__
```Swift
class Solution {
    func removeElement(_ nums: inout [Int], _ val: Int) -> Int {
        var end = 0
        for i in stride(from: 0, to: nums.count, by: 1) {
            if nums[i] != val {
                nums.swapAt(i, end)
                end+=1
            }
        }
        return end
    }
}
```