
### Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (xn).

__Example 1:__
```
Input: 2.00000, 10
Output: 1024.00000
```
__Example 2:__
```
Input: 2.10000, 3
Output: 9.26100
```
__Example 3:__
```
Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```
__Note:__

* -100.0 < x < 100.0
* n is a 32-bit signed integer, within the range [−231, 231 − 1]

### Solution
__O(n):__
```Swift
class Solution {
    func myPow(_ x: Double, _ n: Int) -> Double {
        var result : Double = 1
        let pow = abs(n)
        if n == 0 {
            return result
        }
        for _ in 1...pow {
            result*=x
        }
        return n > 0 ? result : 1/result
    }
}
```
__O(logn):__
```Swift
class Solution {
    func myPow(_ x: Double, _ n: Int) -> Double {
        return n >= 0 ? pow(x, n) : 1/pow(x, n)
    }
    
    func pow(_ x: Double, _ n: Int) -> Double {
        switch n {
            case 0:
            return 1
            case 1:
            return x
            default:
            let result = pow(x, n/2)
            switch n%2 {
                case 0:
                return result*result
                default:
                return x*result*result
            }
        }
    }
}
```