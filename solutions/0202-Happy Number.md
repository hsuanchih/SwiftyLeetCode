
### Happy Number

Write an algorithm to determine if a number `n` is happy.

A __happy number__ is a number defined by the following process:
* Starting with any positive integer, replace the number by the sum of the squares of its digits.
* Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
* Those numbers for which this process ends in 1 are happy.

Return `true` if `n` is a happy number, and `false` if not.

__Example 1:__
```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```
__Example 2:__
```
Input: n = 2
Output: false
```

__Constraints:__
* `1 <= n <= pow(2, 31) - 1`

### Solution
__Iterative:__
```Swift
class Solution {
    func isHappy(_ n: Int) -> Bool {
        var seen: Set<Int> = []
        var n: Int = n
        while n != 1 && !seen.contains(n) {
            seen.insert(n)
            var temp: Int = n
            var num: Int = 0
            while temp > 0 {
                let digit: Int = temp % 10
                num += digit * digit
                temp /= 10
            }
            n = num
        }
        return n == 1
    }
}
```
__Recursive:__
```Swift
class Solution {
    func isHappy(_ n: Int) -> Bool {
        var seen: Set<Int> = []
        return isHappy(n, &seen)
    }

    func isHappy(_ n: Int, _ seen: inout Set<Int>) -> Bool {
        if n == 1 { 
            return true 
        } else if seen.contains(n) {
            return false
        } else {
            seen.insert(n)
            var n: Int = n
            var num: Int = 0
            while n > 0 {
                let digit: Int = n % 10
                num += digit * digit
                n /= 10
            }
            return isHappy(num, &seen)
        }
    }
}
```