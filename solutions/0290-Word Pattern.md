
### Word Pattern

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here __follow__ means a full match, such that there is a bijection between a letter in `pattern` and a __non-empty__ word in str.

__Example 1:__
```
Input: pattern = "abba", str = "dog cat cat dog"
Output: true
```
__Example 2:__
```
Input:pattern = "abba", str = "dog cat cat fish"
Output: false
```
__Example 3:__
```
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false
```
__Example 4:__
```
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
```
__Notes:__
You may assume pattern contains only lowercase letters, and str contains lowercase letters that may be separated by a single space.

### Solution
__O(pattern+str) Time, O(pattern+str) Space:__
```Swift
class Solution {
    func wordPattern(_ pattern: String, _ str: String) -> Bool {
        let pattern = Array(pattern), str = str.split(separator: " ").map(String.init)
        if pattern.count != str.count {
            return false
        }
        var lookup : [Character : String] = [:], seen : Set<String> = []
        for i in stride(from: 0, to: pattern.count, by: 1) {
            let key = pattern[i], value = str[i]
            if let word = lookup[key] {
                if word != value {
                    return false
                }
            } else {
                if seen.contains(value) {
                    return false
                }
                lookup[key] = value
                seen.insert(value)
            }
        }
        return true
    }
}
```