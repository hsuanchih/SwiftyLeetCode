
### Find the Index of the First Occurrence in a String

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

__Example 1:__
```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```
__Example 2:__
```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
```

__Constraints:__
* `1 <= haystack.length, needle.length <= pow(10, 4)`
* `haystack` and `needle` consist of only lowercase English characters.

### Solution
__O(haystack * needle) Time:__
```Swift
class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else { return -1 }
        let haystack: [Character] = Array(haystack)
        let needle: [Character] = Array(needle)
        for i in 0 ..< haystack.count where haystack[i] == needle[0] {
            for j in 0 ..< needle.count {
                if i + j >= haystack.count || haystack[i + j] != needle[j] {
                    break
                }
                if j == needle.count - 1 {
                    return i
                }
            }
        }
        return -1
    }
}
```
__O(haystack * needle) Time:__
```Swift
class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !haystack.isEmpty, !needle.isEmpty, needle.count <= haystack.count else { return -1 }
        let hayStack: [Character] = Array(haystack)
        let needle: ArraySlice<Character> = Array(needle)[0 ..< needle.count]
        for i in 0 ... haystack.count - needle.count {
            if hayStack[i ..< i + needle.count] == needle {
                return i
            }
        }
        return -1
    }
}
```