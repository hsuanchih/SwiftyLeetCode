
### Shortest Unsorted Continuous Subarray

Given an integer array, you need to find one __continuous subarray__ that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the __shortest__ such subarray and output its length.

__Example 1:__
```
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```

__Note:__
1. Then length of the input array is in range `[1, 10,000]`.
2. The input array may contain duplicates, so ascending order here means __<=__.

### Solution
__O(nums\*log(nums)+nums) Time, O(nums) Space:__
```Swift
class Solution {
    func findUnsortedSubarray(_ nums: [Int]) -> Int {
        let sorted = nums.sorted()
        var i = Int.min, j = Int.min
        for k in 0..<nums.count {
            if nums[k] != sorted[k] && i == Int.min {
                i = k
            }
            if nums[nums.count-1-k] != sorted[nums.count-1-k] && j == Int.min {
                j = nums.count-1-k
            }
            if i != Int.min && j != Int.min {
                break
            }
        }
        return j == i ? 0 : j-i+1
    }
}
```