
### Perfect Squares

Given a positive integer *n*, find the least number of perfect square numbers</br> 
(for example, `1, 4, 9, 16, ...`) which sum to *n*.

__Example 1:__
```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.
```
__Example 2:__
```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

### Solution
__Exponential Time Recursive Top-Down:__
```Swift
class Solution {
    func numSquares(_ n: Int) -> Int {

        // Base case:
        // n == 0: solution = 0
        // n == 1: solution = 1
        if n <= 1 {
            return n
        }

        // Recurring sub-problem:
        // Given number n, compute the minimum number of squares
        // required to make up the number:
        // Try all squares smaller than sqrt(n), calling numSquares
        // on the difference to find the minimum
        var min = n
        for num in 1...Int(Double(n).squareRoot()) {
            min = Swift.min(min, numSquares(n-num*num))
        }
        return min+1
    }
}
```
__Polynomial Time Recursive Top-Down:__
```Swift
class Solution {
    func numSquares(_ n: Int) -> Int {

        // Same solution as recusrive top-down, but with memoization
        var memo : [Int?] = Array(repeating: nil, count: n+1)
        return numSquares(n, &memo)
    }
    
    func numSquares(_ n: Int, _ memo: inout [Int?]) -> Int {
        if n <= 1 {
            return n
        }
        if let result = memo[n] {
            return result
        }
        var min = n
        for num in 1...Int(Double(n).squareRoot()) {
            min = Swift.min(min, numSquares(n-num*num, &memo))
        }
        memo[n] = min+1
        return memo[n]!
    }
}
```
__Polynomial Time Iterative Bottom-Up:__
```Swift
class Solution {
    func numSquares(_ n: Int) -> Int {

        // The max number of squares making up a number n
        // will be n: (1+1+1+1+1...)
        var memo : [Int] = Array(0...n)

        // Topological order:
        // 1. Staring with num = 0, compute the minimum number of squares needed to make up
        //    num using all squares smaller than num
        // 2. Work our way up to num = n
        for num in 0...n {
            for i in 0...Int(sqrt(Double(num))) {
                memo[num] = min(memo[num], 1+memo[num-i*i])
            }
        }
        return memo.last ?? 0
    }
}
```