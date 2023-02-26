
### Find First and Last Position of Element in Sorted Array

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

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
__Example 3:__
```
Input: nums = [], target = 0
Output: [-1,-1]
```

__Constraints:__
* `0 <= nums.length <= 10^5`
* `-10^9 <= nums[i] <= 10^9`
* `nums` is a non-decreasing array.
* `-10^9 <= target <= 10^9`

### Solution
__O(nums) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func searchRange(_ nums: [Int], _ target: Int) -> [Int] {
        var result : [Int] = [-1, -1]
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
__O(2*log\[base 2\](nums)) Time, O(1) Space - Binary Search:__
```Swift
class Solution {
    func searchRange(_ nums: [Int], _ target: Int) -> [Int] {

        var result: [Int] = [-1, -1]
        guard !nums.isEmpty else { return result }

        // Find the starting position of target
        var start: Int = 0, end = nums.count - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            if nums[mid] >= target {
                end = mid
            } else {
                start = mid
            }
        }
        if nums[start] == target {
            result[0] = start
        } else if nums[end] == target {
            result[0] = end
            start = end
        } else {
            return result
        }
        
        // Find the ending position of target
        end = nums.count - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            if nums[mid] > target {
                end = mid
            } else {
                start = mid
            }
        }
        if nums[end] == target {
            result[1] = end
        } else if nums[start] == target {
            result[1] = start
        }
        return result
    }
}
```