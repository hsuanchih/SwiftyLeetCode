
### Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found.</br> 
If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

__Example 1:__
```
Input: nums = [1,3,5,6], target = 5
Output: 2
```
__Example 2:__
```
Input: nums = [1,3,5,6], target = 2
Output: 1
```
__Example 3:__
```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

__Constraints:__
* `1 <= nums.length <= 10^4`
* `-10^4 <= nums[i] <= 10^4`
* `nums` contains __distinct__ values sorted in __ascending__ order.
* `-10^4 <= target <= 10^4`

### Solution
__O(nums) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func searchInsert(_ nums: [Int], _ target: Int) -> Int {
        for index in stride(from: 0, to: nums.count, by: 1) {
            switch nums[index] {
                case target...Int.max:
                return index
                default:
                break
            }
        }
        return nums.count
    }
}
```
__O(log\[base 2\](nums)) Time, O(1) Space - Binary Search:__
```Swift
class Solution {
    func searchInsert(_ nums: [Int], _ target: Int) -> Int {
        var start: Int = 0, end: Int = nums.count - 1
        while start + 1 < end {
            let mid = start + (end - start) / 2
            switch nums[mid] {
            case target:
                return mid
            case Int.min ..< target:
                start = mid
            case target + 1 ... Int.max:
                end = mid
            default:
                fatalError()
            }
        }
        if nums[end] < target {
            return end + 1
        }
        if nums[start] < target {
            return start + 1
        }
        return start
    }
}
```