
### Climbing Stairs

You are climbing a stair case. It takes *n* steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

__Note:__ Given *n* will be a positive integer.

__Example 1:__
```
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```
__Example 2:__
```
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

### Solution
__O(2^n):__
```Swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        if n <= 2 {
            return n
        }
        return climbStairs(n-1)+climbStairs(n-2)
    }
}
```
__O(n) Time + O(n) Space (Top-Down):__
```Swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        var memo : [Int?] = Array(repeating: nil, count: n+1)
        return numWays(n, &memo)
    }
    
    func numWays(_ n: Int, _ memo: inout [Int?]) -> Int {
        if let ways = memo[n] {
            return ways
        }
        memo[n] = n <= 2 ? n : numWays(n-1, &memo)+numWays(n-2, &memo)
        return memo[n]!
    }
}
```
__O(n) Time + O(n) Space (Bottom-Up):__
```Swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        var memo : [Int] = Array(repeating: 1, count: n+1)
        for stair in stride(from: 2, through: n, by: 1) {
            memo[stair] = memo[stair-1] + memo[stair-2]
        }
        return memo.last!
    }
}
```
__O(n) Time + O(1) Space (Bottom-Up):__
```Swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        var minus1 = 1, minus2 = 1, result = 1
        for stair in stride(from: 2, through: n, by: 1) {
            result = minus1 + minus2
            minus2 = minus1
            minus1 = result
        }
        return result
    }
}
```