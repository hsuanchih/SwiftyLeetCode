
### Find First and Last Position of Element in Sorted Array

Given an array of integers nums sorted in ascending order,</br> 
find the starting and ending position of a given target value.</br>

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return `[-1, -1]`.

__Example 1:__
```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```
__Example 2:__
```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

### Solution
__O(n):__
```Swift
class Solution {
    func searchRange(_ nums: [Int], _ target: Int) -> [Int] {
        var result : [Int] = [-1,-1]
        for index in stride(from: 0, to: nums.count, by: 1) {
            switch nums[index] {
                case target:
                if result.first == -1 {
                    result[0] = index
                }
                result[1] = index
                default:
                break
            }
        }
        return result
    }
}
```
__O(2*logn):__
```Swift
class Solution {
    func searchRange(_ nums: [Int], _ target: Int) -> [Int] {

        var result : [Int] = [-1, -1]
        if nums.isEmpty {
            return result
        }

        // Find the starting position of target
        var start = 0, end = nums.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            if nums[mid] >= target {
                end = mid
            } else {
                start = mid
            }
        }
        switch (nums[start], nums[end]) {
            case (target, _):
            result[0] = start
            case (_, target):
            result[0] = end
            default:
            return result
        }
        
        // Find the ending position of target
        start = 0 
        end = nums.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            if nums[mid] <= target {
                start = mid
            } else {
                end = mid
            }
        }
        switch (nums[start], nums[end]) {
            case (_, target):
            result[1] = end
            case (target, _):
            result[1] = start
            default:
            result[1] = result[0]
        }
        return result
    }
}
```