
### Ransom Note

Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

__Example 1:__
```
Input: ransomNote = "a", magazine = "b"
Output: false
```
__Example 2:__
```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```
__Example 3:__
```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

__Constraints:__
* `1 <= ransomNote.length, magazine.length <= pow(10, 5)`
* `ransomNote` and `magazine` consist of lowercase English letters.

### Solution
__O(ransomNote + magazine):__
```Swift
class Solution {
    func canConstruct(_ ransomNote: String, _ magazine: String) -> Bool {
        // Keep count of each character in the magazine
        var lookup: [Character: Int] = magazine.reduce(into: [Character: Int]()) { lookup, char in
            lookup[char, default: 0] += 1
        }

        // Iterate through ransomNote, keep count of remaining characters in magazine 
        // if any character to use in ransom note is no longer available in magazine, return false
        for char in ransomNote {
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
        return true
    }
}
```