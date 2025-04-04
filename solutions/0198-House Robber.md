
### House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and __it will automatically contact the police if two adjacent houses were broken into on the same night__.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight __without alerting the police__.

__Example 1:__
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```
__Example 2:__
```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

__Constraints:__
* `1 <= nums.length <= 100`
* `0 <= nums[i] <= 400`

### Solution
__O(pow(2, nums)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        maxProfit(nums, 0)
    }

    func maxProfit(_ nums: [Int], _ house: Int) -> Int {
        guard house < nums.count else { return 0 }
        return max(nums[house] + maxProfit(nums, house + 2), maxProfit(nums, house + 1))
    }
}
```
__O(nums) Time, O(nums) Space - Recursive + Memoization:__
```Swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        var memo: [Int?] = Array(repeating: nil, count: nums.count)
        return maxProfit(nums, 0, &memo)
    }

    func maxProfit(_ nums: [Int], _ index: Int, _ memo: inout [Int?]) -> Int {
        guard index < nums.count else { return 0 }
        if let profit = memo[index] {
            return profit
        } else {
            memo[index] = max(
                nums[index] + maxProfit(nums, index + 2, &memo), 
                maxProfit(nums, index + 1, &memo)
            )
            return memo[index]!
        }
    }
}
```
__O(nums) Time, O(nums) Space - Bottom-Up Iterative, Bottom-Up Memoization:__
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