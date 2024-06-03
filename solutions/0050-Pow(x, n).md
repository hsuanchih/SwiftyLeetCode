
### Pow(x, n)

Implement `pow(x, n)`, which calculates `x` raised to the power `n` (i.e., `pow(x, n)`).

__Example 1:__
```
Input: x = 2.00000, n = 10
Output: 1024.00000
```
__Example 2:__
```
Input: x = 2.10000, n = 3
Output: 9.26100
```
__Example 3:__
```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

__Constraints:__
* `-100.0 < x < 100.0`
* `-pow(2, 31) <= n <= pow(2, 31) - 1`
* `n` is an integer.
* Either `x` is not zero or `n > 0`.
* `-pow(10, 4) <= pow(x, n) <= pow(10, 4)`

### Solution
__O(n) Time - Brute-Force:__
```Swift
class Solution {
    func myPow(_ x: Double, _ n: Int) -> Double {
        if n == 0 || x == 1 { return 1 }
        let isNegativePower: Bool = n < 0
        let n: Int = abs(n)
        var num: Double = 1
        for _ in 1 ... n {
            num *= x
        }
        return isNegativePower ? 1 / num : num
    }
}
```
__O(log\[2\](n)) Time - Even-Odd:__
```Swift
class Solution {
    func myPow(_ x: Double, _ n: Int) -> Double {
        n >= 0 ? pow(x, n) : 1 / pow(x, abs(n))
    }

    func pow(_ x: Double, _ n: Int) -> Double {
        if n == 0 || x == 1 { return 1 }
        let result: Double = pow(x, n / 2)
        return n % 2 == 0 ? result * result : result * result * x
    }
}
```