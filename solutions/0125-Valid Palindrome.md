
### Valid Palindrome

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

__Note:__ For the purpose of this problem, we define empty string as valid palindrome.

__Example 1:__
```
Input: "A man, a plan, a canal: Panama"
Output: true
```
__Example 2:__
```
Input: "race a car"
Output: false
```

### Solution
__O(n):__
```Swift
class Solution {
    func isPalindrome(_ s: String) -> Bool {
        let s = Array(s)
        var start = 0, end = s.count-1
        while start < end {
            if !(s[start].isNumber || s[start].isLetter) {
                start+=1
                continue
            }
            if !(s[end].isNumber || s[end].isLetter) {
                end-=1
                continue
            }
            if s[start].lowercased() != s[end].lowercased() {
                return false
            }
            start+=1
            end-=1
        }
        return true
    }
}
```