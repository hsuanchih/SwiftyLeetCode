
### Reverse Integer

Given a 32-bit signed integer, reverse digits of an integer.

__Example 1:__
```
Input: 123
Output: 321
```
__Example 2:__
```
Input: -123
Output: -321
```
__Example 3:__
```
Input: 120
Output: 21
```
__Note:__

Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1].</br> 
For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

### Solution
__String Manipulation:__
```Swift
extension Int {
    var converted : Int {
        switch self {
            case (1<<31)...Int.max, Int.min..<(-(1<<31)):
            return 0
            default:
            return self
        }
    }
}

class Solution {
    func reverse(_ x: Int) -> Int {
        let sign = x < 0 ? -1 : 1
        return (sign*Int(String(String(abs(x)).reversed()))!).converted
    }
}
```
__O(log\[base 10\](x) Time, O(1) Space - Iterative:__
```Swift
extension Int {
    var converted : Int {
        switch self {
            case (1<<31)...Int.max, Int.min..<(-(1<<31)):
            return 0
            default:
            return self
        }
    }
}

class Solution {
    func reverse(_ x: Int) -> Int {
        let sign = x < 0 ? -1: 1
        var x = abs(x), result = 0
        while x > 0 {
            result = result*10+x%10
            x/=10
        }
        return sign*result.converted
    }
}
```
__O(log\[base 10\](x) Time, O(1) Space - Recursive:__
```Swift
extension Int {
    var converted : Int {
        switch self {
            case (1<<31)...Int.max, Int.min..<(-(1<<31)):
            return 0
            default:
            return self
        }
    }
}

class Solution {
    func reverse(_ x: Int) -> Int {
        let sign = x < 0 ? -1 : 1
        return (sign*Int(reversing(abs(x)))!).converted
    }
    
    func reversing(_ x: Int) -> String {
        guard x >= 10 else { return String(x) }
        return String(x%10).appending(reversing(x/10))
    }
}
```