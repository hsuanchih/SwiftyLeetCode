
### Valid Palindrome

A phrase is a __palindrome__ if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a __palindrome__, or `false` otherwise.

__Example 1:__
```
Input: "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```
__Example 2:__
```
Input: "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```
__Example 3:__
```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

__Constraints:__
* `1 <= s.length <= 2 * pow(10, 5)`
* `s` consists only of printable ASCII characters.

### Solution
__O(s) Time, O(1) Space:__
```Swift
class Solution {
    func isPalindrome(_ s: String) -> Bool {
        let s: [Character] = Array(s)
        var start: Int = 0, end: Int = s.count - 1
        while start < end {
            if !s[start].isLetter, !s[start].isNumber {
                start += 1
                continue
            }
            if !s[end].isLetter, !s[end].isNumber {
                end -= 1
                continue
            }
            if s[start].lowercased() != s[end].lowercased() {
                return false
            } else {
                start += 1
                end -= 1
            }
        }
        return true
    }
}
```