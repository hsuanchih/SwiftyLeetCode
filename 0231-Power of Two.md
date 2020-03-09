
### Power of Two

Given an integer, write a function to determine if it is a power of two.

__Example 1:__
```
Input: 1
Output: true 
Explanation: 20 = 1
```
__Example 2:__
```
Input: 16
Output: true
Explanation: 24 = 16
```
__Example 3:__
```
Input: 218
Output: false
```

### Solution
__O(log\[base2\](n)) Recursive:__
```Swift
class Solution {
    func isPowerOfTwo(_ n: Int) -> Bool {
        switch n {
            case Int.min...0: return false
            case 1: return true
            default:
            if n%2 == 0 {
                return isPowerOfTwo(n/2)
            }
            return false
        }
    }
}
```
__O(log\[base2\](n)) Iterative:__
```Swift
class Solution {
    func isPowerOfTwo(_ n: Int) -> Bool {
        switch n {
            case Int.min...0: return false
            case 1: return true
            default:
            var n = n
            while n > 1 {
                if n%2 == 0 {
                    n/=2
                } else {
                    return false
                }
            }
            return true
        }
    }
}
```
__O(1):__
```Swift
class Solution {
    func isPowerOfTwo(_ n: Int) -> Bool {
        return n > 0 && n&(n-1) == 0
    }
}
```