
### Valid Anagram

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An __Anagram__ is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

__Example 1:__
```
Input: s = "anagram", t = "nagaram"
Output: true
```
__Example 2:__
```
Input: s = "rat", t = "car"
Output: false
```

__Constraints:__
* `1 <= s.length, t.length <= 5 * pow(10, 4)`
* `s` and `t` consist of lowercase English letters.

__Follow up:__ 
* What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

### Solution
__O(s + t) Time, O(s) Space:__
```Swift
class Solution {
    func isAnagram(_ s: String, _ t: String) -> Bool {
        var lookup: [Character: Int] = s.reduce(into: [Character: Int]()) { lookup, char in
            lookup[char, default: 0] += 1
        }

        for char in t {
            if let count = lookup[char] {
                if count > 1 {
                    lookup[char] = count - 1
                } else {
                    lookup.removeValue(forKey: char)
                }
            } else {
                return false
            }
        }
        return lookup.isEmpty
    }
}
```