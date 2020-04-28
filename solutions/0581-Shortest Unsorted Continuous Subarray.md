
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
__O(nums\*log\[base 2\](nums)+nums) Time, O(nums) Space - Sort Input:__
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
__O(nums) Time, O(nums) Space - Stack:__
```Swift
class Solution {
    func findUnsortedSubarray(_ nums: [Int]) -> Int {
        var stack : [Int] = []
        var start = nums.count, end = 0
        for (i, num) in nums.enumerated() {
            while let last = stack.last, nums[last] > num {
                start = min(start, stack.removeLast())
            }
            stack.append(i)
        }
        stack.removeAll()
        for i in stride(from: nums.count-1, through: 0, by: -1) {
            while let last = stack.last, nums[last] < nums[i] {
                end = max(end, stack.removeLast())
            }
            stack.append(i)
        }
        return max(end-start+1, 0)
    }
}
```
__O(nums) Time, O(1) Space - Min & Max Index After Unsorted Pivot:__
```Swift
class Solution {
    func findUnsortedSubarray(_ nums: [Int]) -> Int {
        if nums.count <= 1 {
            return 0
        }
        var minVal = Int.max
        for i in 1..<nums.count {
            if minVal == Int.max {
                if nums[i] < nums[i-1] {
                    minVal = nums[i]
                }
            } else {
                minVal = min(minVal, nums[i])
            }
        }
        var maxVal = Int.min
        for i in stride(from: nums.count-2, through: 0, by: -1) {
            if maxVal == Int.min {
                if nums[i] > nums[i+1] {
                    maxVal = nums[i]
                }
            } else {
                maxVal = max(maxVal, nums[i])
            }
        }
        var left = nums.count
        for (i, num) in nums.enumerated() {
            if num > minVal {
                left = i
                break
            }
        }
        var right = 0
        for i in stride(from: nums.count-1, through: 0, by: -1) {
            if nums[i] < maxVal {
                right = i
                break
            }
        }
        return max(0, right-left+1)
    }
}
```