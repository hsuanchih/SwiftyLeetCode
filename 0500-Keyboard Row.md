
### Keyboard Row

Given a List of words, return the words that can be typed using letters of __alphabet__ on only one row's of American keyboard like the image below.

![Example](images/question_500.png)

__Example:__
```
Input: ["Hello", "Alaska", "Dad", "Peace"]
Output: ["Alaska", "Dad"]
```

__Note:__
1. You may use one character in the keyboard more than once.
2. You may assume the input string will only contain letters of alphabet.

### Solution
__O(words*k+26) Time, O(26) Space:__
```Swift
class Solution {
    func findWords(_ words: [String]) -> [String] {
        
        var lookup : [String: Int] = [:]
        "qwertyuiop".forEach { lookup[String($0)] = 0 }
        "asdfghjkl".forEach { lookup[String($0)] = 1 }
        "zxcvbnm".forEach { lookup[String($0)] = 2 }
        
        var result : [String] = []
        for word in words {
            guard !word.isEmpty else { continue }
            let index = lookup[word.first!.lowercased()]!
            var isSameRow = true
            for char in word {
                if lookup[char.lowercased()]! != index {
                    isSameRow = false
                    break
                }
            }
            if isSameRow {
                result.append(word)
            }
        }
        return result
    }
}
```