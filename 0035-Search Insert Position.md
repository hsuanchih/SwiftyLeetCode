
### Search Insert Position

Given a sorted array and a target value, return the index if the target is found.</br>
If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

__Example 1:__
```
Input: [1,3,5,6], 5
Output: 2
```
__Example 2:__
```
Input: [1,3,5,6], 2
Output: 1
```
__Example 3:__
```
Input: [1,3,5,6], 7
Output: 4
```
__Example 4:__
```
Input: [1,3,5,6], 0
Output: 0
```

### Solution
__O(n):__
```Swift
class Solution {
    func searchInsert(_ nums: [Int], _ target: Int) -> Int {
        var start = 0, end = nums.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            switch nums[mid] - target {
                case 0:
                return mid
                case Int.min..<0:
                start = mid
                default:
                end = mid
            }
        }
        if nums[end] < target {
            return end+1
        }
        if nums[start] < target {
            return start+1
        }
        return start
    }
}
```