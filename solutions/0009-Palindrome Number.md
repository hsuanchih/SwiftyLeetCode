
### Palindrome Number

Given an integer `x`, return `true` if `x` is a palindrome, and `false` otherwise.

__Example 1:__
```
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
```
__Example 2:__
```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```
__Example 3:__
```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

__Constraints:__
* `-pow(2, 31) <= x <= pow(2, 31) - 1`
 
__Follow up:__ 
* Could you solve it without converting the integer to a string?

### Solution
__String Manipulation:__
```Swift
class Solution {
    func isPalindrome(_ x: Int) -> Bool {
        let x: [Character] = Array(String(x))
        var start: Int = 0, end: Int = x.count - 1
        while start < end {
            if x[start] != x[end] {
                return false
            }
            start += 1
            end -= 1
        }
        return true
    }
}
```
__Iterative:__
```Swift
class Solution {
    func isPalindrome(_ x: Int) -> Bool {
        guard x >= 0 else { return false }
        var num: Int = x
        var reversed: Int = 0
        while num > 0 {
            reversed = reversed * 10 + (num % 10)
            num /= 10
        }
        return x == reversed
    }
}
```