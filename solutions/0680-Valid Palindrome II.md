
### Valid Palindrome II

Given a non-empty string `s`, you may delete __at most__ one character. Judge whether you can make it a palindrome.

__Example 1:__
```
Input: "aba"
Output: True
```
__Example 2:__
```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

__Note:__
* The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

### Solution
__O(s) Time:__
```Swift
class Solution {
    func validPalindrome(_ s: String) -> Bool {
        let s = Array(s)
        var start = 0, end = s.count-1
        while start < end {

          // If we encounter characters that don't match, check if substring from the mismatch
          // forward from either end is a palindrome (ie, delete one character)
          if s[start] != s[end] {
            return validPalindrome(s[start+1...end]) || validPalindrome(s[start...end-1])
          }

          // Otherwise continue checking the original string
          start+=1
          end-=1
        }
        return true
    }
    
    func validPalindrome(_ s: ArraySlice<Character>) -> Bool {
        var start = s.startIndex, end = s.endIndex-1
        while start < end {
            if s[start] != s[end] {
                return false
            }
            start+=1
            end-=1
        }
        return true
    }
}
```