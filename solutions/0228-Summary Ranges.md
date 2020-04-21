
### Summary Ranges

Given a sorted integer array without duplicates, return the summary of its ranges.

__Example 1:__
```
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```
__Example 2:__
```
Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```

### Solution
__O(nums) Time:__
```Swift
class Solution {
    func summaryRanges(_ nums: [Int]) -> [String] {
        var start = 0, result : [String] = []
        for end in stride(from: 0, to: nums.count, by: 1) {
            if end == nums.count-1 || nums[end]+1 != nums[end+1] {
                result.append(generateRange(nums, start, end))
                start = end+1
            }
        }
        return result
    }
    
    func generateRange(_ nums: [Int], _ start: Int, _ end: Int) -> String {
        if start == end {
            return String(nums[start])
        }
        return "\(nums[start])->\(nums[end])"
    }
}
```