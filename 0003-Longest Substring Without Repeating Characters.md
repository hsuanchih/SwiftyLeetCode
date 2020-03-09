
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
__O(n^2):__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        let s = Array(s)
        
        var seen : Set<Character> = [], 
        result : Int = 0
        
        // Evaluate each substring starting at every "start"
        for start in stride(from: 0, to: s.count, by: 1) {
            seen.removeAll()
            for index in stride(from: start, through: s.count, by: 1) {
                
                // If we've reached the end of the substring or if duplicate is found, update result & go to the next "start" 
                if index == s.count || seen.contains(s[index]) {
                    result = max(result, index-start)
                    break
                }
                
                // Otherwise mark the current character as seen
                seen.insert(s[index])
            }
        }
        return result
    }
}
```
__O(n):__
```Swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        let s = Array(s)
        
        var seen : Set<Character> = [], 
        longest : Int = 0,
        start : Int = 0, 
        end : Int = 0
        
        while end < s.count {
            
            // If a duplicate is found: 
            // update result, mark character s[start] as un-seen & advance start
            // Otherwise:
            // mark the current character as seen & continue with traversal 
            if seen.contains(s[end]) {
                longest = max(longest, end-start)
                seen.remove(s[start])
                start+=1
            } else {
                seen.insert(s[end])
                end+=1
            }
        }
        
        // Update result for the last time in case we reach end without running into duplicates again
        return max(longest, end-start)
    }
}
```