
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
class Solution {
    func reverse(_ x: Int) -> Int {
        let num = abs(x), result = Int(String(String(num).reversed()))!
        
        // Account for 32-bit signed integer restriction
        switch (x, result) {
            case (Int.min..<0, (1<<31)+1...Int.max), (0...Int.max, (1<<31)...Int.max):
            return 0
            case (Int.min..<0, _):
            return -result
            default:
            return result
        }
    }
}
```
__Iterative:__
```Swift
class Solution {
    func reverse(_ x: Int) -> Int {
        var num = abs(x), result = 0
        while num > 0 {
            result = result*10 + num%10
            num/=10
        }

        // Account for 32-bit signed integer restriction
        switch (x, result) {
            case (Int.min..<0, (1<<31)+1...Int.max), (0...Int.max, (1<<31)...Int.max):
            return 0
            case (Int.min..<0, _):
            return -result
            default:
            return result
        }
    }
}
```