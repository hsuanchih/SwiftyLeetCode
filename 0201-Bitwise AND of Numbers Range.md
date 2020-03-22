
### Bitwise AND of Numbers Range

Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

__Example 1:__
```
Input: [5,7]
Output: 4
```
__Example 2:__
```
Input: [0,1]
Output: 0
```

### Solution
__O(n-m+1) Time, O(1) Space:__
```Swift
class Solution {
    class Solution {
    func rangeBitwiseAnd(_ m: Int, _ n: Int) -> Int {
        guard m < n else { return m }
        var result = m
        for num in m+1...n {
            result&=num
        }
        return result
    }
}
}
```
__O(n-m+1) Time, O(1) Space - Pruned:__
```Swift
class Solution {
    func rangeBitwiseAnd(_ m: Int, _ n: Int) -> Int {
        guard m < n else { return m }
        var result = m
        for num in m+1...n {
            result&=num
            if result == 0 {
                return result
            }
        }
        return result
    }
}
```
__O(log(max(m,n))) Time, O(1) Space - Bit Manipulation:__
```Swift
class Solution {
    func rangeBitwiseAnd(_ m: Int, _ n: Int) -> Int {
        var m = m, n = n, shifts = 0

        // We're trying to find the left of bits that are common amongst m & n
        // as the variations to the right of the common bits are going to zero
        // each other out when an AND operation is applied to the range from m to n.
        // Once the common left half is found, m will be equal to n
        while m != n {
            m >>= 1
            n >>= 1
            shifts += 1
        }
        return m << shifts
    }
}
```