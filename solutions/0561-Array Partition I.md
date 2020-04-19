
### Array Partition I

Given an array of __2n__ integers, your task is to group these integers into __n__ pairs of integer,</br> 
say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

__Example 1:__
```
Input: [1,4,3,2]

Output: 4
Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
```

__Note:__
1. __n__ is a positive integer, which is in the range of `[1, 10000]`.
2. All the integers in the array will be in the range of `[-10000, 10000]`.

### Solution
__O(n*log(n)+n) Time:__
```Swift
class Solution {
    func arrayPairSum(_ nums: [Int]) -> Int {
        let nums = nums.sorted()
        var result = 0
        for index in stride(from: 0, to: nums.count-1, by: 2) {
            result+=min(nums[index], nums[index+1])
        }
        return result
    }
}
```