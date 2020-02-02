
### Longest Palindromic Substring

Given a string __s__, find the longest palindromic substring in __s__.</br> 
You may assume that the maximum length of __s__ is 1000.

__Example 1:__
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```
__Example 2:__
```
Input: "cbbd"
Output: "bb"
```

### Solution
__O(n^3):__
```Swift
class Solution {
    func longestPalindrome(_ s: String) -> String {
        let s = Array(s)
        var result : Range<Int> = 0..<0

        // Evaluate substrings of different lengths
        for start in stride(from: 0, to: s.count, by: 1) {
            for end in stride(from: start, to: s.count, by: 1) {

                // Check whether Characters on both ends of the substring match
                // Iterate inwards to the center
                var i = start, j = end
                while i < j && s[i] == s[j] {
                    i+=1
                    j-=1
                }

                // If start & end meet in the middle, the substring is a palindrome
                // Update result if substring is longer than previously recorded
                if i >= j && end-start+1 > result.count {
                    result = start..<end+1
                }
            }
        }
        return result.isEmpty ? "" : String(s[result])
    }
}
```
__O(n^2):__
```Swift
class Solution {
    func longestPalindrome(_ s: String) -> String {
        let s = Array(s)

        // Dynamic programming:
        // dp[start][end] memoizes whether s[start][end] is a palindrome
        var dp : [[Bool]] = Array(repeating: Array(repeating: false, count: s.count), count: s.count),
        result : Range<Int> = 0..<0

        // Start with substring of size 1 & expand into substring of size n
        for end in stride(from: 0, to: s.count, by: 1) {
            for start in stride(from: 0, through: end, by: 1) {
                if s[start] == s[end] {

                    // If the indices are the same, then the substring is a single character -> a palindrome
                    // If the indices are one unit apart, then the substring is a palindrome if the 2 characters are the same
                    // If the indices are more than one unit apart, then the substring is a palindrome only if the substring between the indices is a palindrome
                    if start == end || start == end-1 {
                        dp[start][end] = true
                    } else {
                        dp[start][end] = dp[start+1][end-1]
                    }

                    // Update result if the current palindrome is longer than previously recorded
                    if dp[start][end] && end-start+1 > result.count {
                        result = start..<end+1
                    }
                }
            }
        }
        return result.isEmpty ? "" : String(s[result])
    }
}
```
__O(n^2):__
```Swift
class Solution {
    func longestPalindrome(_ s: String) -> String {
        let s = Array(s)
        var result : Range<Int> = 0..<0

        // Recursive solution that checks for substrings expanding from each of 2n+1 centers
        for index in stride(from: 0, to: s.count, by: 1) {
            
            // A single character is a palindrome
            if result.isEmpty {
                result = index..<index+1
            }

            // Check for longest palindrome expanding from (index, index+1)
            solve(s, index, index+1, &result)

            // Check for longest palindrome expanding from index
            solve(s, index-1, index+1, &result)
        }
        return result.isEmpty ? "" : String(s[result])
    }
    
    func solve(_ s: [Character], _ start: Int, _ end: Int, _ result: inout Range<Int>) {
        
        // Abort when indices go out of bounds
        guard start >= 0 && end < s.count else {
            return
        }

        // If characters on both ends match, update result accordingly & extend search range
        if s[start] == s[end] {
            if end-start+1 > result.count {
                result = start..<end+1
            }
            solve(s, start-1, end+1, &result)
        }
    }
}
```