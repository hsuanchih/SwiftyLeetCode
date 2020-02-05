
### Combination Sum

Given a __set__ of candidate numbers (candidates) __(without duplicates)__ and a target number (target),</br> 
find all unique combinations in candidates where the candidate numbers sums to target.

The __same__ repeated number may be chosen from candidates unlimited number of times.

__Note:__

* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

__Example 1:__
```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```
__Example 2:__
```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

### Solution
__O(2^n):__
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