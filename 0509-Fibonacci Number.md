
### Fibonacci Number

The __Fibonacci numbers__, commonly denoted `F(n)` form a sequence, called the __Fibonacci sequence__,</br> 
such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,
```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1.
```
Given `N`, calculate `F(N)`.

__Example 1:__
```
Input: 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.
```
__Example 2:__
```
Input: 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.
```
__Example 3:__
```
Input: 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
```

### Solution
__O(2^n) Time, O(1) Space:__
```Swift
class Solution {
    func fib(_ N: Int) -> Int {
        if N <= 1 {
            return N
        }
        return fib(N-1) + fib(N-2)
    }
}
```
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func fib(_ N: Int) -> Int {
        var memo : [Int] = Array(repeating: 0, count: N+1)
        for n in 0...N {
            if n <= 1 {
                memo[n] = n
            } else {
                memo[n] = memo[n-1] + memo[n-2]
            }
        }
        return memo.last!
    }
}
```
__O(n) Time, O(1) Space:__
```Swift
class Solution {
    func fib(_ N: Int) -> Int {
        if N <= 1 {
            return N
        }
        var minus2 = 0, minus1 = 1
        for n in 2...N {
            let curr = minus1 + minus2
            minus2 = minus1
            minus1 = curr
        }
        return minus1
    }
}
```