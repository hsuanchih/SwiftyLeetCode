
### Sqrt(x)

Implement `int sqrt(int x)`.

Compute and return the square root of *x*, where *x* is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

__Example 1:__
```
Input: 4
Output: 2
```
__Example 2:__
```
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
```

### Solution
__O(x):__
```Swift
class Solution {
    func mySqrt(_ x: Int) -> Int {
        for root in 0...x {
            switch root*root {
                case x:
                return root
                case x+1...Int.max:
                return root-1
                default:
                break
            }
        }
        fatalError()
    }
}
```
__O(logx):__
```Swift
class Solution {
    func mySqrt(_ x: Int) -> Int {
        var start = 0, end = x
        while start+1 < end {
            let mid = start + (end-start)/2
            switch mid*mid {
                case x:
                return mid
                case 0..<x:
                start = mid
                default:
                end = mid
            }
        }
        return end*end > x ? start : end
    }
}
```