
### Find Peak Element

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

__Example 1:__
```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```
__Example 2:__
```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

__Note:__
Your solution should be in logarithmic complexity.

### Solution
__O(nums) Time - Brute-Force:__
```Swift
class Solution {
    func findPeakElement(_ nums: [Int]) -> Int {
        switch nums.count {
            case 0:
            return -1
            case 1:
            return 0
            default:
            break
        }
        for index in stride(from: 0, to: nums.count, by: 1) {
            switch index {
                case 0:
                if nums[index] > nums[index+1] {
                    return index
                }
                case nums.count-1:
                if nums[index] > nums[index-1] {
                    return index
                }
                default:
                if nums[index] > nums[index-1] && nums[index] > nums[index+1] {
                    return index
                }
            }
        }
        fatalError()
    }
}
```
__O(log\[base 2\](nums)) Time - Binary Search:__
```Swift
class Solution {
    func findPeakElement(_ nums: [Int]) -> Int {
        if nums.isEmpty {
            return -1
        }
        var start = 0, end = nums.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            if nums[mid] > nums[mid-1] {
                if nums[mid] > nums[mid+1] {
                    return mid
                }
                start = mid
            } else {
                end = mid
            }
        }
        return nums[start] < nums[end] ? end : start
    }
}
```