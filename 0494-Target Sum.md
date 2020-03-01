
### Target Sum

You are given a list of non-negative integers, `a1, a2, ..., an`, and a target, `S`.</br> 
Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

__Example 1:__
```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

__Note:__
1. The length of the given array is positive and will not exceed `20`.
2. The sum of elements in the given array will not exceed `1000`.
3. Your output answer is guaranteed to be fitted in a 32-bit integer.

### Solution
__O(2^nums) Time, O(1) Space:__
```Swift
class Solution {
    func findTargetSumWays(_ nums: [Int], _ S: Int) -> Int {
        return compute(nums, 0, S)
    }
    
    func compute(_ nums: [Int], _ index: Int, _ S: Int) -> Int {
        if index == nums.count {
            return S == 0 ? 1 : 0
        }
        return compute(nums, index+1, S-nums[index]) + compute(nums, index+1, S+nums[index])
    }
}
```
__O(Range(sum)\*nums) Time, O(Range(sum)*nums) Space:__
```Swift
class Solution {
    func findTargetSumWays(_ nums: [Int], _ S: Int) -> Int {
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: 1000*2), count: nums.count)
        return compute(nums, 0, S, 0, &memo)
    }
    
    func compute(_ nums: [Int], _ index: Int, _ S: Int, _ sum: Int, _ memo: inout [[Int?]]) -> Int {
        if index == nums.count {
            return sum == S ? 1 : 0
        }
        if let result = memo[index][sum+1000] {
            return result
        }
        memo[index][sum+1000] = compute(nums, index+1, S, sum+nums[index], &memo) + compute(nums, index+1, S, sum-nums[index], &memo)
        return memo[index][sum+1000]!
    }
}
```