
### Length of Last Word

Given a string s consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word (last word means the last appearing word if we loop from left to right) in the string.

If the last word does not exist, return 0.

__Note:__ A word is defined as a __maximal substring__ consisting of non-space characters only.

__Example:__
```
Input: "Hello World"
Output: 5
```

### Solution
__O(s) Time, O(1) Space:__
```Swift
class Solution {
    func lengthOfLastWord(_ s: String) -> Int {
        let s = Array(s)
        var start = s.count-1, end = s.count-1
        while start >= 0 {
            switch s[start] {
                case " ":
                if end == start {

                    // Process trailing spaces
                    end-=1
                } else {
                    
                    // Return length of last word
                    return end-start
                }
                default:
                break
            }
            start-=1
        }
        return end-start
    }
}
```