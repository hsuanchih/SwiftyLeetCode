
### Climbing Stairs

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

__Example 1:__
```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```
__Example 2:__
```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

__Constraints:__
* `1 <= n <= 45`

### Solution
__O(pow(2, n)) Time, Brute-Force:__
```swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        if n == 0 {
            return 1
        } else if n == 1 {
            return climbStairs(n - 1)
        } else {
            return climbStairs(n - 1) + climbStairs(n - 2) 
        }
    }
}
```
__O(n) Time + O(n) Space, Top-Down Recursive + Memoization:__
```swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        var memo: [Int?] = Array(repeating: nil, count: n + 1)
        return climbStairs(n, &memo)
    }

    func climbStairs(_ n: Int, _ memo: inout [Int?]) -> Int {
        let result: Int
        if let ways = memo[n] {
            result = ways
        } else {
            if n == 0 {
                result = 1
            } else if n == 1 {
                result = climbStairs(n - 1, &memo)
            } else {
                result = climbStairs(n - 1, &memo) + climbStairs(n - 2, &memo) 
            }
            memo[n] = result
        }
        return result
    }
}
```
__O(n) Time + O(n) Space, Bottom-Up Iterative + Memoization:__
```swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        var dp: [Int] = Array(repeating: 0, count: n + 1)
        for i in 0 ... n {
            if i == 0 {
                dp[i] = 1
            } else if i == 1 {
                dp[i] = dp[i - 1]
            } else {
                dp[i] = dp[i - 1] + dp[i - 2]
            }
        }
        return dp.last ?? 0
    }
}
```
__O(n) Time + O(1) Space, Bottom-Up Iterative + Memoization Space Optimized:__
```swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        if n <= 1 {
            return 1
        } else {
            var result: Int = 0
            var minus1: Int = 1
            var minus2: Int = 1
            for _ in 2 ... n {
                result = minus1 + minus2
                minus2 = minus1
                minus1 = result
            }
            return result
        }
    }
}
```