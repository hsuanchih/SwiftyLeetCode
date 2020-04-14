
### Ransom Note

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

__Note:__
You may assume that both strings contain only lowercase letters.

```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

### Solution
__O(ransomNote+magazine):__
```Swift
class Solution {
    func canConstruct(_ ransomNote: String, _ magazine: String) -> Bool {

        // Keep count of each character in the magazine
        var lookup = magazine.reduce(into: [Character: Int]()) { $0[$1, default: 0]+=1 }
        
        // Iterate through ransomNote, keep count of remaining characters in magazine 
        // if any character to use in ransom note is no longer available in magazine, return false
        for char in ransomNote {
            guard let count = lookup[char], count > 0 else { return false }
            lookup[char] = count-1
        }
        return true
    }
}
```