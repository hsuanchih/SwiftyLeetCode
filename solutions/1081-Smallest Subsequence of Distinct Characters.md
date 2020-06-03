
### Smallest Subsequence of Distinct Characters

Return the lexicographically smallest subsequence of text that contains all the distinct characters of text exactly once.

__Example 1:__
```
Input: "cdadabcc"
Output: "adbc"
```
__Example 2:__
```
Input: "abcd"
Output: "abcd"
```
__Example 3:__
```
Input: "ecbacba"
Output: "eacb"
```
__Example 4:__
```
Input: "leetcode"
Output: "letcod"
```

__Constraints:__
1. `1 <= text.length <= 1000`
2. `text` consists of lowercase English letters.

__Note:__ This question is the same as 316: [https://leetcode.com/problems/remove-duplicate-letters/](https://leetcode.com/problems/remove-duplicate-letters/)

### Solution
__O(text) Time, O(text) Space:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue! - Character("a").asciiValue!)
    }
}

class Solution {
    func smallestSubsequence(_ text: String) -> String {
        var frequency : [Int] = Array(repeating: 0, count: 26)
        for char in text {
            frequency[char.offset]+=1
        }
        var used : [Bool] = Array(repeating: false, count: 26),
        result : [Character] = []
        for char in text {
            let offset = char.offset
            defer { frequency[offset]-=1 }
            guard !used[offset] else { continue }
            while let last = result.last, last.offset > offset, frequency[last.offset] > 0 {
                _ = result.removeLast()
                used[last.offset] = false
            }
            result.append(char)
            used[offset] = true
        }
        return String(result)
    }
}
```