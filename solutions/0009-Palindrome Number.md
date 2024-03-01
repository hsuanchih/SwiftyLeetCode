
### Palindrome Number

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

__Example 1:__
```
Input: 121
Output: true
```
__Example 2:__
```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```
__Example 3:__
```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```
__Follow up:__

Coud you solve it without converting the integer to a string?

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
        var num = x, result = 0
        while num > 0 {
            result = result*10+num%10
            num/=10
        }
        return x == result
    }
}
```