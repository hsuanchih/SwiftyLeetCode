
### Longest Substring Without Repeating Characters

Given a string `s`, find the length of the longest substring without repeating characters.

__Example 1:__
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```
__Example 2:__
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```
__Example 3:__
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

__Constraints:__
* `0 <= s.length <= 5 * pow(10, 4)`
* `s` consists of English letters, digits, symbols and spaces.

### Solution
__O(pow(s, 3)) Time - Brute-Force, TLE:__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        guard !s.isEmpty else { return 0 }
        var longest: Int = 1
        for start in s.indices {
            for end in s.indices[start ..< s.endIndex] {
                var seen: Set<Character> = []
                for i in s.indices[start ... end] {
                    let letter: Character = s[i]
                    if seen.contains(letter) {
                        longest = max(longest, s.distance(from: start, to: i))
                        break
                    } else {
                        seen.insert(letter)
                        if i == end {
                            longest = max(longest, s.distance(from: start, to: i) + 1)
                        }
                    }
                }
            }
        }
        return longest
    }
}
```
__O(pow(s, 2)) Time - Improved Brute-Force, TLE:__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        guard !s.isEmpty else { return 0 }
        var longest: Int = 1
        for start in s.indices {
            var seen: Set<Character> = []
            for end in s.indices[start ..< s.index(s.startIndex, offsetBy: s.count)] {
                let letter: Character = s[end]
                if seen.contains(letter) {
                    longest = max(longest, s.distance(from: start, to: end))
                    break
                } else {
                    seen.insert(letter)
                    if end == s.index(before: s.endIndex) {
                        longest = max(longest, s.distance(from: start, to: end) + 1)
                    }
                }
            }
        }
        return longest
    }
}
```
__O(s) Time - Sliding Window:__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        guard !s.isEmpty else { return 0 }

        // `start` tracks the start of the current window 
        var start: String.Index = s.startIndex

        // `seen` tracks all the characters in the current window 
        var seen: Set<Character> = []

        // `result` tracks the longest substring
        var longest: Int = 1

        for end in s.indices {
            let letter: Character = s[end]
            if seen.contains(letter) {

                // If we run into a character that already exists in the current window:
                // 1. Update result if the current window is the longest substring
                // 2. Forward the starting point of the sliding window until all characters
                //    in the window are unique
                longest = max(longest, s.distance(from: start, to: end))
                while start < end, s[start] != letter {
                    seen.remove(s[start])
                    start = s.index(after: start)
                }
                start = s.index(after: start)
            } else {
                seen.insert(letter)

                // If we reach the end `s`, we've walked the entire input so return the longest among 
                // the current window and result
                if end == s.index(before: s.endIndex) {
                    longest = max(longest, s.distance(from: start, to: end) + 1)
                }
            }
        }
        return longest
    }
}
```
