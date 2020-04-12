
### Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log *n*).

__Example 1:__
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```
__Example 2:__
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

### Solution
__O(log\[base 2\](nums)) Time, O(1) Space:__
```Swift
// Array Extension
extension Array where Element == Int {

    // Computed property to find pivot (index of the smallest element)
    var pivot : Int {
        if isEmpty {
            return -1
        }

        // Array is in ascending order, return 0
        if first! <= last! {
            return 0
        }

        // Binary search to find the pivot
        switch (binarySearch(0, count-1) { (mid, start, end) in

            // If mid element is greater than array's first element, pivot is to the right,
            // otherwise pivot is to the left
            switch self[mid] {
                case self[0]+1...Int.max:
                start = mid
                default:
                end = mid
            }
        }) {
        case (-1, -1):
            return -1
        case let (start, end):
            return self[start] < self[end] ? start : end
        }
    }
    
    // Find target using binary search
    func binarySearch(_ start: Int, _ end: Int, _ target: Element) -> Int {
        switch (binarySearch(start, end) { (mid, start, end) in
            
            // If mid element is target, end search immediately
            // If mid element is smaller than target, search to the right of mid
            // otherwise search to the left of mid
            switch self[mid] {
            case target:
                start = mid
                end = mid
            case Int.min..<target:
                start = mid
            default:
                end = mid
            }
        }) {
        case (-1, -1):
            return -1
        case let (start, end):
            switch (self[start], self[end]) {
                case (target, _):
                return start
                case (_ , target):
                return end
                default:
                return -1
            }
        }
    }
    
    // Binary search helper method
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
    func search(_ nums: [Int], _ target: Int) -> Int {
        
        // Find the pivot in the array
        let pivot = nums.pivot

        // If pivot is -1, we have an empty array, return -1
        // If pivot is 0, array is not rorated, binary search from 0 to nums.count-1 for target
        switch pivot {
            case -1:
            return -1
            case 0:
            return nums.binarySearch(0, nums.count-1, target)
            default:
            break
        }

        // If array is rorated & target is greater than array's first element, target is to the left of pivot
        // otherwise target is to right of pivot
        if target >= nums[0] {
            return nums.binarySearch(0, pivot-1, target)
        } else {
            return nums.binarySearch(pivot, nums.count-1, target)
        }
    }
}
```