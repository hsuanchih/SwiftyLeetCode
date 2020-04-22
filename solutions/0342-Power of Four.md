
### Power of Four

Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

__Example 1:__
```
Input: 16
Output: true
```
__Example 2:__
```
Input: 5
Output: false
```

__Follow up:__ Could you solve it without loops/recursion?

### Solution
__O(log\[base4\](num)) Recursive:__
```Swift
class Solution {
    func isPowerOfFour(_ num: Int) -> Bool {
        switch num {
            case Int.min...0: return false
            case 1: return true
            default:
            if num%4 == 0 {
                return isPowerOfFour(num/4)
            }
            return false
        }
    }
}
```
__O(log\[base4\](num)) Iterative:__
```Swift
class Solution {
    func isPowerOfFour(_ num: Int) -> Bool {
        switch num {
            case Int.min...0: return false
            case 1: return true
            default:
            var num = num
            while num%4 == 0 {
                num/=4
            }
            return num == 1
        }
    }
}
```
__O(1):__
```Swift
class Solution {
    func isPowerOfFour(_ num: Int) -> Bool {

        // num <= 0: Cannot be power of 4
        // num & (num-1) == 0: Validates that num is power of 2
        // (num-1)%3 == 0: If num is power of 2 & num-1 is divisible by 3, then num is power of 4
        return num > 0 && num & (num-1) == 0 && (num-1)%3 == 0
    }
}
```