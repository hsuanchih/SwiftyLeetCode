
### Range Sum Query - Immutable

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

__Example:__
```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

__Note:__
1. You may assume that the array does not change.
2. There are many calls to *sumRange* function.

### Solution
```Swift
class NumArray {

    // prefixSum tracks the running sum of the input array
    var prefixSum : [Int]

    // O(n)
    init(_ nums: [Int]) {
        prefixSum = Array(repeating: 0, count: nums.count)
        for (index, num) in nums.enumerated() {
            if index == 0 {
                prefixSum[index] = nums[index]
            } else {
                prefixSum[index] = nums[index] + prefixSum[index-1]
            }
        }
    }
    
    // O(1)
    func sumRange(_ i: Int, _ j: Int) -> Int {
        if i == 0 {
            return prefixSum[j]
        } else {
            return sumRange(0, j) - sumRange(0, i-1)
        }
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * let obj = NumArray(nums)
 * let ret_1: Int = obj.sumRange(i, j)
 */
```