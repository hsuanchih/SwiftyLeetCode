
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
        if n <= 1 {
            return n
        }
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
        var memo : [Int] = Array(0...n)
        for i in 0...n {
            for num in 0...i/2 {
                let square = num*num
                if i-square < 0 { break }
                memo[i] = min(memo[i], memo[i-square]+1)
            }
        }
        return memo.last!
    }
}
```