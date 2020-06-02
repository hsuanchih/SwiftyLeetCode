
### Remove Duplicate Letters

Given a string which contains only lowercase letters, remove duplicate letters so that every letter appears once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

__Example 1:__
```
Input: "bcabc"
Output: "abc"
```
__Example 2:__
```
Input: "cbacdcbc"
Output: "acdb"
```

__Note:__ This question is the same as 1081: [https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

### Solution
__O(s) Time, O(s) Space:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue! - Character("a").asciiValue!)
    }
}
class Solution {
    func removeDuplicateLetters(_ s: String) -> String {

        // Use a hashmap to track the number of ocurrences of each character in s
        var frequency : [Int] = Array(repeating: 0, count: 26)
        for char in s {
            frequency[char.offset]+=1
        }

        var used : Set<Character> = [], result : [Character] = []
        // Iterate through each character in s
        for char in s {

            // Unconditionally track the remaining count of each character as we go
            defer { frequency[char.offset]-=1 }

            // If current character has been used earlier, skip this character
            guard !used.contains(char) else { continue }

            // Otherwise, we want to pop characters in our result that are lexicographically larger
            // than the current character - if we know that they will occur again in the future
            while let last = result.last, last.offset > char.offset, frequency[last.offset] > 0 {
                _ = result.removeLast()
                used.remove(last)
            }

            // add the current character to the result & mark it as used
            result.append(char)
            used.insert(char)
        }
        return String(result)
    }
}
```