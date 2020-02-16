
### Implement strStr()

Implement `strStr()`.
Return the index of the first occurrence of needle in haystack, or __-1__ if needle is not part of haystack.

__Example 1:__
```
Input: haystack = "hello", needle = "ll"
Output: 2
```
__Example 2:__
```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```
__Clarification:__
What should we return when `needle` is an empty string? This is a great question to ask during an interview.
For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's strstr() and Java's `indexOf()`.

### Solution
__O(n*k):__
```Swift
class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        switch (haystack.isEmpty, needle.isEmpty) {
            case (true, false):
            return -1
            case (false, true), (true, true):
            return 0
            case (false, false):
            break
        }
        if needle.count > haystack.count {
            return -1
        }
        let hayStack = Array(haystack), needle = Array(needle)[0..<needle.count]
        for i in 0...haystack.count-needle.count {
            if hayStack[i..<i+needle.count] == needle {
                return i
            }
        }
        return -1
    }
}
```