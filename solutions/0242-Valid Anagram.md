
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

__Note:__
You may assume the string contains only lowercase alphabets.

__Follow up:__
What if the inputs contain unicode characters? How would you adapt your solution to such case?

### Solution
__O(s+t) Time, O(s) Space:__
```Swift
class Solution {
    func isAnagram(_ s: String, _ t: String) -> Bool {
        var sMap = s.reduce(into: [Character: Int]()) { $0[$1, default: 0]+=1 }
        for char in t {
            if let count = sMap[char] {
                if count > 1 {
                    sMap[char] = count-1
                } else {
                    sMap.removeValue(forKey: char)
                }
            } else {
                return false
            }
        }
        return sMap.isEmpty
    }
}
```