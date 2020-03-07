
### Ugly Number

Write a program to check whether a given number is an ugly number.

Ugly numbers are __positive numbers__ whose prime factors only include `2, 3, 5`.

__Example 1:__
```
Input: 6
Output: true
Explanation: 6 = 2 × 3
```
__Example 2:__
```
Input: 8
Output: true
Explanation: 8 = 2 × 2 × 2
```
__Example 3:__
```
Input: 14
Output: false 
Explanation: 14 is not ugly since it includes another prime factor 7.
```

__Note:__
1. `1` is typically treated as an ugly number.
2. Input is within the 32-bit signed integer range: `[−2^31,  2^31 − 1]`.

### Solution
__Recursive:__
```Swift
class Solution {
    func isUgly(_ num: Int) -> Bool {
        switch num {
            case Int.min...0:
            return false
            case 1:
            return true
            default:
            for factor in [2,3,5] {
                if num%factor == 0 {
                    return isUgly(num/factor)
                }
            }
        }
        return false
    }
}
```
__Iterative:__
```Swift
class Solution {
    func isUgly(_ num: Int) -> Bool {
        switch num {
            case Int.min...0:
            return false
            case 1:
            return true
            default:
            var num = num
            while num > 1 {
                for factor in [2,3,5] {
                    if num%factor == 0 {
                        num/=factor
                        break
                    }
                    if factor == 5 {
                        return false
                    }
                }
            }
            return true
        }
    }
}
```