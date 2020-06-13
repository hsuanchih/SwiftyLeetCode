
### Palindromic Substrings

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

__Example 1:__
```
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```
__Example 2:__
```
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

__Note:__
1. The input string length won't exceed `1000`.

### Solution
__O(s^3) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func countSubstrings(_ s: String) -> Int {
        let s = Array(s)
        var result = 0

        // Examine each substring from start to end & check
        // if the substring is a palindrome
        for start in 0..<s.count {
            for end in start..<s.count {
                var left = start, right = end
                while left < right {
                    if s[left] != s[right] {
                        break
                    }
                    left+=1
                    right-=1
                }
                if left >= right {
                    result+=1
                }
            }
        }
        return result
    }
}
```
__O(s^2) Time, O(s^2) Space - Bottom-Up Iterative, Bottom-Up Memoization:__
```Swift
class Solution {
    func countSubstrings(_ s: String) -> Int {
        let s = Array(s)
        var memo : [[Bool]] = Array(repeating: Array(repeating: false, count: s.count), count: s.count)
        var result = 0

        // Topological order is to begin with substring of length 1 starting at each index
        // and expand into substrings of all lengths
        // Length 1: we have a single character & it must be a palindrome
        // Length 2: Substring is a palindrome iff the 2 characters are equal
        // Other lengths: Substring is a palindrome if s[start] == s[start+length], and
        //                substring s[start...start+length-1] is also a palindrome
        for length in 0..<s.count {
            for start in 0..<s.count where start+length < s.count {
                switch length {
                    case 0:
                    memo[start][start+length] = true
                    case 1:
                    memo[start][start+length] = s[start] == s[start+length]
                    default:
                    memo[start][start+length] = s[start] == s[start+length] && memo[start+1][start+length-1]
                }
                if memo[start][start+length] {
                    result+=1
                }
            }
        }
        return result
    }
}
```