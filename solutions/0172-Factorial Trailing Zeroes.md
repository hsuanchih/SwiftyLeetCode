
### Factorial Trailing Zeroes

Given an integer `n`, return the number of trailing zeroes in `n!`.

Note that `n! = n * (n - 1) * (n - 2) * ... * 3 * 2 * 1`.

__Example 1:__
```
Input: n = 3
Output: 0
Explanation: 3! = 6, no trailing zero.
```
__Example 2:__
```
Input: n = 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```
__Example 3:__
```
Input: n = 0
Output: 0
```

__Constraints:__
* `0 <= n <= pow(10, 4)`

__Follow up:__ 
* Could you write a solution that works in logarithmic time complexity?

### Solution
__n + n * log\[10\](n) Time - Brute-Force, Overflow:__
```Swift
class Solution {
    func trailingZeroes(_ n: Int) -> Int {
        guard n > 0 else { return 0 }
        var num = (1 ... n).reduce(into: 1, *=)
        var zeros: Int = 0
        while num / 10 > 0, num % 10 == 0 {
            zeros += 1
            num /= 10
        }
        return zeros
    }
}
```
__(n / 5) * log\[5\](n) Time - Optimized:__
```Swift
class Solution {
    func trailingZeroes(_ n: Int) -> Int {
        guard n > 0 else { return 0 }
        var num: Int = 5
        var zeros: Int = 0
        while num <= n {
            var temp: Int = num
            while temp % 5 == 0 {
                temp /= 5
                zeros += 1
            }
            num += 5
        }
        return zeros
    }
}
```
__log\[5\](n) Time - Optimized:__
```Swift
class Solution {
    func trailingZeroes(_ n: Int) -> Int {
        var num: Int = n
        var zeros: Int = 0
        while num > 0 {
            zeros += num / 5
            num /= 5
        }
        return zeros
    }
}
```