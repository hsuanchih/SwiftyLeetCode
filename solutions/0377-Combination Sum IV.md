
### Combination Sum IV

Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

__Example:__
```
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

__Follow up:__
* What if negative numbers are allowed in the given array?
* How does it change the problem?
* What limitation we need to add to the question to allow negative numbers?

__Credits:__
Special thanks to @pbrother for adding this problem and creating all test cases.

### Solution
__O(nums^target) Time, O(1) Space - Exhaustive Search:__
```Swift
class Solution {
    func combinationSum4(_ nums: [Int], _ target: Int) -> Int {
        var result : Int = 0
        solve(nums, target, &result)
        return result
    }
    
    func solve(_ nums: [Int], _ target: Int, _ result: inout Int) {
        if target == 0 {
            result+=1
            return
        }
        for num in nums where num <= target {
            solve(nums, target-num, &result)
        }
    }
}
```
__O(nums*target) Time, O(target) Space - Top-Down Recursive, Bottom-Up Memoization:__
```Swift
class Solution {
    func combinationSum4(_ nums: [Int], _ target: Int) -> Int {
        var memo: [Int?] = Array(repeating: nil, count: target+1)
        return solve(nums, target, &memo)
    }
    
    func solve(_ nums: [Int], _ target: Int, _ memo: inout [Int?]) -> Int {
        if target == 0 {
            return 1
        }
        if let result = memo[target] {
            return result
        }
        var sum = 0
        for num in nums where num <= target {
            sum += solve(nums, target-num, &memo)
        }
        memo[target] = sum
        return memo[target]!
    }
}
```
__O(nums*target) Time, O(target) Space - Bottom-Up Iterative, Bottom-Up Memoization:__
```Swift
class Solution {
    func combinationSum4(_ nums: [Int], _ target: Int) -> Int {
        var memo: [Int?] = Array(repeating: nil, count: target+1)
        return solve(nums, target, &memo)
    }
    
    func solve(_ nums: [Int], _ target: Int, _ memo: inout [Int?]) -> Int {
        if target == 0 {
            return 1
        }
        if let result = memo[target] {
            return result
        }
        var sum = 0
        for num in nums where num <= target {
            sum += solve(nums, target-num, &memo)
        }
        memo[target] = sum
        return memo[target]!
    }
}
```