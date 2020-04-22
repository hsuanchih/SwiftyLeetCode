
### Power of Three

Given an integer, write a function to determine if it is a power of three.

__Example 1:__
```
Input: 27
Output: true
```
__Example 2:__
```
Input: 0
Output: false
```
__Example 3:__
```
Input: 9
Output: true
```
__Example 4:__
```
Input: 45
Output: false
```

__Follow up:__
Could you do it without using any loop / recursion?

### Solution
__O(log\[base 3\](n) Time - Iterative:__
```Swift
class Solution {
    func isPowerOfThree(_ n: Int) -> Bool {
        switch n {
            case 0:
            return false
            case var n:
            while n%3 == 0 {
                n/=3
            }
            return n == 1
        }
    }
}
```
__O(log\[base 3\](n) Time - Recursive:__
```Swift
class Solution {
    func isPowerOfThree(_ n: Int) -> Bool {
        switch n {
            case 0:
            return false
            case 1:
            return true
            case let n:
            if n%3 == 0 {
                return isPowerOfThree(n/3)
            }
            return false
        }
    }
}
```