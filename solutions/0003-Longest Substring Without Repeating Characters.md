
### Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

__Example 1:__
```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```
__Example 2:__
```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```
__Example 3:__
```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Solution
__O(s^3) Time, Constant (26) Space - Brute-Force:__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        let s: [Character] = Array(s)
        var result: Int = 0
        for start in 0 ..< s.count {
            for end in start ... s.count {
                var seen: Set<Character> = []

                // Evaluate every subset of s
                // ie, from every start to every end
                // to find the longest subset without repeating characters
                for i in start ... end { 
                    if i == end || seen.contains(s[i]) {
                        result = max(result, i-start)
                        break
                    }
                    seen.insert(s[i])
                }
            }
        }
        return result
    }
}
```
__O(s^2) Time, Constant (26) Space - Improved Brute-Force:__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        let s: [Character] = Array(s)
        var result: Int = 0
        
        // Evaluate each substring starting at every "start"
        for start in 0 ..< s.count {
            var seen: Set<Character> = []
            for i in start ... s.count {
                
                // Evaluate each substring starting at every "start"
                if i == s.count || seen.contains(s[i]) {
                    result = max(result, i-start)
                    break
                }
                
                // Otherwise mark the current character as seen
                seen.insert(s[i])
            }
        }
        return result
    }
}
```
__O(s^2) Time, Constant (26) Space - Improved Brute-Force Alternative:__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        let s: [Character] = Array(s)
        var result: Int = 0
        
        // Evaluate each substring starting at every "start"
        for start in 0 ..< s.count {
            var seen: Set<Character> = []
            for i in start ..< s.count {
                // Evaluate each substring starting at every "start"
                let curr: Character = s[i]
                if seen.contains(curr) {
                    break
                }
                result = max(result, i-start+1)
                seen.insert(curr)
            }
        }
        return result
    }
}
```
__O(s) Time, Constant (26) Space - Sliding Window:__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        let s: [Character] = Array(s)
        var seen: Set<Character> = [], result = 0
        var start: Int = 0, end: Int = 0
        
        // Sliding window
        while end <= s.count {
            
            // If a duplicate is found or if end has reached the end of s
            if end == s.count || seen.contains(s[end]) {
                
                // Update result
                result = max(result, end-start)
                
                // If the end of s is reached, break out of the loop
                if end == s.count { break }
                
                // Otherwise mark character at s[start] as un-seen & advance start
                seen.remove(s[start])
                start += 1
                continue
            }
            
            // Duplicate is not found, 
            // mark the current character as seen & continue with traversal
            seen.insert(s[end])
            end += 1
        }
        return result
    }
}
```
