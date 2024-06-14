
### Length of Last Word

Given a string `s` consisting of words and spaces, return the length of the last word in the string.

A word is a maximal substring consisting of non-space characters only.

__Example 1:__
```
Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.
```
__Example 2:__
```
Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.
```
__Example 3:__
```
Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.
```
 
__Constraints:__
* `1 <= s.length <= pow(10, 4)`
* `s` consists of only English letters and spaces `' '`.
* There will be at least one word in `s`.

### Solution
__O(s) Time, O(1) Space:__
```Swift
class Solution {
    func lengthOfLastWord(_ s: String) -> Int {
        var end: String.Index = s.index(before: s.endIndex)
        while end >= s.startIndex, s[end] == " " {
            end = s.index(before: end)
        }

        var start: String.Index? = end
        while let index = start, index >= s.startIndex, s[index] != " " {
            start = s.index(index, offsetBy: -1, limitedBy: s.startIndex)
        }
        if let start {
            return s.distance(from: start, to: end)
        } else {
            return s.distance(from: s.startIndex, to: end) + 1
        }
    }
}
```