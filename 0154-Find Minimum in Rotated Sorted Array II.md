
### Find Minimum in Rotated Sorted Array II

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

The array may contain duplicates.

__Example 1:__
```
Input: [1,3,5]
Output: 1
```
__Example 2:__
```
Input: [2,2,2,0,1]
Output: 0
```

__Note:__
* This is a follow up problem to Find Minimum in Rotated Sorted Array.
* Would allow duplicates affect the run-time complexity? How and why?

### Solution
__O(n):__
```Swift
class Solution {
    func findMin(_ nums: [Int]) -> Int {
        return nums.reduce(Int.max) { min($1, $0) }
    }
}
```