
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
__O(pow(s, 3)) Time, O(1) Space - Exhaustive Search:__
```Swift
class Solution {
    func longestPalindrome(_ s: String) -> String {
        guard !s.isEmpty else { return s }
        let s = Array(s)
        var result: ClosedRange<Int> = 0 ... 0

        // Exhaustive search of every subarray to see if each subarray is a palindrome
        // Evaluate substrings of different lengths
        for start in 0 ..< s.count {
            // Update result if substring is a palindrome & longer than previously recorded
            for end in start ..< s.count where isPalindrome(s, start ... end) && end - start + 1 > result.count {
                result = start ... end
            }
        }
        return String(s[result])
    }

    func isPalindrome(_ s: [Character], _ range: ClosedRange<Int>) -> Bool {
        var start: Int = range.lowerBound, end: Int = range.upperBound

        // Check whether Characters on both ends of the substring match
        // Iterate inwards to the center
        while start < end {
            if s[start] != s[end] {
                return false
            }
            start += 1
            end -= 1
        }

        // If start & end meet in the middle, the substring is a palindrome
        return true
    }
}

```
__O(pow(s, 2)) Time, O(pow(s, 2)) Space - Bottom-Up:__
```Swift
class Solution {
    func longestPalindrome(_ s: String) -> String {
        guard !s.isEmpty else { return s }
        let s: [Character] = Array(s)

        // Dynamic programming:
        // memo[start][end] memoizes whether s[start][end] is a palindrome
        var memo: [[Bool]] = Array(repeating: Array(repeating: false, count: s.count), count: s.count)
        var result: ClosedRange<Int> = 0 ... 0

        for end in 0 ..< s.count {
            for start in 0 ... end where s[start] == s[end] {

                // If the indices are the same, then the substring is a single character -> a palindrome
                // If the indices are one unit apart, then the substring is a palindrome if the 2 characters are the same
                // If the indices are more than one unit apart, then the substring is a palindrome only if the substring between the indices is a palindrome
                if start == end || start + 1 == end {
                    memo[start][end] = true
                } else {
                    memo[start][end] = memo[start + 1][end - 1]
                }

                // Update result if the current palindrome is longer than previously recorded
                if memo[start][end], end - start + 1 > result.count {
                    result = start ... end
                }
            }
        }
        return String(s[result])
    }
}
```
__O((2n+1)*n) Time, O(1) Space - Middle-Out:__
```Swift
class Solution {

    func longestPalindrome(_ s: String) -> String {

        // If length of s is less than or equal to 1, return s
        if s.count <= 1 { return s }
        let s = Array(s)
        var result : Range<Int> = 0 ..< 0

        // Iterate through each character of s & expand out from s[i]
        // to find the range of the longest palindrome extending from s[i]
        for i in 0 ..< s.count-1 {

            // A single character is by definition a palindrome
            // Find longest palindrome extending from s[i]
            let longest = longestPalindrome(s, i-1, i+1)
            if longest.count > result.count {
                result = longest
            }

            // If s[i] == s[i+1], then we also need to check the longest palindrome
            // extending from s[i...i+1]
            if s[i] == s[i+1] {
                let longest = longestPalindrome(s, i-1, i+2)
                if longest.count > result.count {
                    result = longest
                }
            }
        }
        return String(s[result])
    }
    
    // Helper method to check longest palindrome extending from indices (start, end)
    func longestPalindrome(_ s: [Character], _ start: Int, _ end: Int) -> Range<Int> {
        switch (start, end) {

        // If start & end are within bounds of s and s[start] == s[end], 
        // we want to continue explore the longest palindrome
        case (0 ..< s.count, 0 ..< s.count) where s[start] == s[end]:
            return longestPalindrome(s, start-1, end+1)

        // Otherwise the longest palindrome ended at [start+1...end-1]
        default:
            return start+1 ..< end
        }
    }
}
```
