
### Happy Number

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

__Example:__
```
Input: 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

### Solution
__Iterative:__
```Swift
class Solution {
    func isHappy(_ n: Int) -> Bool {
        var n = n, seen : Set<Int> = []
        while n != 1 {
            if seen.contains(n) {
                return false
            } else {
                seen.insert(n)
                var temp = 0
                while n > 0 {
                    let digit = n%10
                    temp+=digit*digit
                    n/=10
                }
                n = temp
            }
        }
        return true
    }
}
```
__Recursive:__
```Swift
class Solution {
    func isHappy(_ n: Int) -> Bool {
        var seen : Set<Int> = []
        return isHappy(n, &seen)
    }
    
    func isHappy(_ n: Int, _ seen: inout Set<Int>) -> Bool {
        if n == 1 {
            return true
        }
        if seen.contains(n) {
            return false
        }
        seen.insert(n)
        var n = n, temp = 0
        while n > 0 {
            let digit = n%10
            temp+=digit*digit
            n/=10
        }
        return isHappy(temp, &seen)
    }
}
```