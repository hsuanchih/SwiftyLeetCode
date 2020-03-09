
### Valid Anagram

Given two strings *s* and *t* , write a function to determine if *t* is an anagram of *s*.

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