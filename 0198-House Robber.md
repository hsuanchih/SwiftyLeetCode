
### House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and __it will automatically contact the police if two adjacent houses were broken into on the same night__.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight __without alerting the police__.

__Example 1:__
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```
__Example 2:__
```
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

### Solution
__O(2^n):__
```Swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        return rob(nums, 0)
    }
    
    func rob(_ nums: [Int], _ index: Int) -> Int {
        if index >= nums.count {
            return 0
        }
        return max(rob(nums, index+1), nums[index]+rob(nums, index+2))
    }
}
```
__Recursive O(n):__
```Swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        var memo : [Int?] = Array(repeating: nil, count: nums.count)
        return rob(nums, 0, &memo)
    }
    
    func rob(_ nums: [Int], _ index: Int, _ memo: inout [Int?]) -> Int {
        if index >= nums.count {
            return 0
        }
        if let result = memo[index] {
            return result
        }
        memo[index] = max(rob(nums, index+1, &memo), nums[index]+rob(nums, index+2, &memo))
        return memo[index]!
    }
}
```
__Iterative O(n):__
```Swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        var memo : [Int] = Array(repeating: 0, count: nums.count)
        for index in stride(from: 0, to: nums.count, by: 1) {
            switch index {
            case 0:
                memo[index] = nums[index]
            case 1:
                memo[index] = max(nums[index], nums[index-1])
            default:
                memo[index] = max(memo[index-1], nums[index]+memo[index-2])
            }
        }
        return memo.last ?? 0
    }
}
```