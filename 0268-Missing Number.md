
### Missing Number

Given an array containing *n* distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.

__Example 1:__
```
Input: [3,0,1]
Output: 2
```
__Example 2:__
```
Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```
__Note:__
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

### Solution
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        var seen : [Bool] = Array(repeating: false, count: nums.count+1)
        nums.forEach { seen[$0] = true }
        return seen.firstIndex(of: false)!
    }
}
```
__O(n) Time, O(1) Space:__
```Swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        return nums.count*(nums.count+1)/2 - nums.reduce(0) { $0+$1 }
    }
}
```