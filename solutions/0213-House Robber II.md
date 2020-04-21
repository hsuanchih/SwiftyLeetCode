
### House Robber II

You are a professional robber planning to rob houses along a street.</br>
Each house has a certain amount of money stashed. All houses at this place are __arranged in a circle__.</br> 
That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and</br> 
__it will automatically contact the police if two adjacent houses were broken into on the same night__.

Given a list of non-negative integers representing the amount of money of each house,</br> 
determine the maximum amount of money you can rob tonight __without alerting the police__.

__Example 1:__
```
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.
```
__Example 2:__
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

### Solution
__O(nums) Time, O(nums) Space:__
```Swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        switch nums.count {
        case 0: return 0
        case 1: return nums.first!
        default: break
        }
        return max(rob(nums, 0, nums.count-1), rob(nums, 1, nums.count))
    }
    
    func rob(_ nums: [Int], _ start: Int, _ end: Int) -> Int {
        var memo : [Int] = Array(repeating: 0, count: nums.count), result = Int.min
        for index in stride(from: start, to: end, by: 1) {
            switch index {
            case start:
                memo[index] = nums[index]
            case start+1:
                memo[index] = max(nums[index], nums[index-1])
            default:
                memo[index] = max(memo[index-2]+nums[index], memo[index-1])
            }
            result = max(result, memo[index])
        }
        return result
    }
}
```