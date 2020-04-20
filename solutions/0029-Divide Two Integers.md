
### Divide Two Integers

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero.

__Example 1:__
```
Input: dividend = 10, divisor = 3
Output: 3
```
__Example 2:__
```
Input: dividend = 7, divisor = -3
Output: -2
```
__Note:__
* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.

### Solution
```Swift
class Solution {
    func divide(_ dividend: Int, _ divisor: Int) -> Int {
        var isPositive : Bool = true, result : UInt64 = 0
        switch (dividend, divisor) {
            case (0...Int.max, Int.min..<0), (Int.min..<0, 0...Int.max):
            isPositive = false
            default:
            break
        }
        return processOverflow(
            isPositive: isPositive, 
            result: computeResult(UInt64(abs(dividend)), UInt64(abs(divisor)))
        )
    }
    
    func computeResult(_ dividend: UInt64, _ divisor: UInt64) -> UInt64 {
        var dividend = dividend, result : UInt64 = 0
        if divisor == 1 {
            return dividend
        } else {
            while dividend >= divisor {
                var shift : UInt64 = 0
                while divisor << shift <= dividend {
                    shift+=1
                }
                result |= 1<<(shift-1)
                dividend -= divisor<<(shift-1)
            }
        }
        return result
    }
    
    func processOverflow(isPositive: Bool, result: UInt64) -> Int {
        switch (isPositive, result) {
            case (true, 1<<31...UInt64.max):
            return 1<<31-1
            case (false, 1<<31+1...UInt64.max):
            return -(1<<31)
            default:
            return isPositive ? Int(result) : -Int(result)
        }
    }
}
```