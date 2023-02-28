
### Sqrt(x)

Given a non-negative integer `x`, return _the square root of_ `x` _rounded down to the nearest integer_.</br>
The returned integer should be __non-negative__ as well.

You __must not use__ any built-in exponent function or operator.

* For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

__Example 1:__
```
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
```
__Example 2:__
```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```

__Constraints:__
* `0 <= x <= 2^31 - 1`

### Solution
__O(x) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func mySqrt(_ x: Int) -> Int {
        for root in 0 ... x {
            switch root * root {
            case x:
                return root
            case x + 1 ... Int.max:
                return root - 1
            default:
                break
            }
        }
        fatalError()
    }
}
```
__O(log\[\base 2](x)) Time, O(1) Space - Binary Search:__
```Swift
class Solution {
    func mySqrt(_ x: Int) -> Int {
        var start: Int = 0, end: Int = x
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            switch mid * mid {
            case x:
                return mid
            case 0 ..< x:
                start = mid
            case x + 1 ... Int.max:
                end = mid
            default:
                fatalError()
            }
        }
        return end * end <= x ? end : start
    }
}
```