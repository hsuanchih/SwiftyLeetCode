
### Find Minimum in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

You may assume no duplicate exists in the array.

__Example 1:__
```
Input: [3,4,5,1,2] 
Output: 1
```
__Example 2:__
```
Input: [4,5,6,7,0,1,2]
Output: 0
```

### Solution
__O(nums) Time - Brute-Force:__
```Swift
class Solution {
    func findMin(_ nums: [Int]) -> Int {
        return nums.reduce(Int.max) { min($1, $0) }
    }
}
```
__O(log\[base 2\](nums)) Time - Binary Search:__
```Swift
extension Array {
    func binarySearch(_ start: Int, _ end: Int, _ transform: (Int, inout Int, inout Int)->Void) -> (start: Int, end: Int) {
        guard start >= 0 && end >= 0 else { return (-1, -1) }
        var start = start, end = end
        while start+1 < end {
            transform(start+(end-start)/2, &start, &end)
        }
        return (start, end)
    }
}

class Solution {
    func findMin(_ nums: [Int]) -> Int {
        if nums.isEmpty {
            return Int.min
        }
        if nums.first! <= nums.last! {
            return nums.first!
        }
        switch (nums.binarySearch(0, nums.count-1) { (mid, start, end) in
            switch nums[mid] {
                case nums.first!+1...Int.max:
                start = mid
                default:
                end = mid
            }
        }) {
            case (-1, -1):
            return -1
            case let (start, end):
            return nums[start] < nums[end] ? nums[start] : nums[end]
        }
    }
}
```