
### Regular Expression Matching

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.
```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```
The matching should cover the entire input string (not partial).

__Note:__
* `s` could be empty and contains only lowercase letters `a-z`.
* `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

__Example 1:__
```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```
__Example 2:__
```
Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```
__Example 3:__
```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```
__Example 4:__
```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```
__Example 5:__
```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

### Solution
__O(3^max(s,p)+(s+p)) Time, O(1) Space - Bottom-Up Recursion:__
```Swift
class Solution {
    func isMatch(_ s: String, _ p: String) -> Bool {
        if s.isEmpty || p.isEmpty {
            return s.isEmpty && p.isEmpty
        }
        return match(Array(s), Array(p), 0, 0)
    }
    
    func match(_ s: [Character], _ p: [Character], _ i: Int, _ j: Int) -> Bool {
        
        // Base case
        switch (i, j) {
            // If s & p both reach the end, then we have a match
            case (s.count, p.count):
            return true
            
            // Process trailing wildcard in p if s reaches the end.
            // s: ""
            // p: "c*c*"
            case (s.count, _):
            return (j+1 < p.count && p[j+1] == "*") && match(s,p,i,j+2,&memo)
            
            // If p reaches the end before s, it's definitely not a match
            case (_, p.count...Int.max):
            return false
            
            default:
            break
        }
        
        switch p[j] {
            
            // If s[i] matches p[j]: same character or ".", check the following cases:
            case s[i], ".":
            // If the next character in p is a wildcard "*", there are 3 cases to consider:
            // 1. Wildcard represents 0 occurence of p[j], match(s,p,i,j+2)
            // 2. Wildcard represents 1 occurence of p[j], match(s,p,i+1,j+2)
            // 3. Wildcard represents multiple occurences of p[j], match(s,p,i+1,j)
            if (j+1<p.count && p[j+1] == "*") {
                return match(s,p,i,j+2) || match(s,p,i+1,j+2) || match(s,p,i+1,j)
            }
            // If the next character in p is not a wildcard "*", 
            // continue to match the next characters
            return match(s,p,i+1,j+1)
            
            // If the characters do not match:
            // If the next character in p is a wildcard, 
            // it can represent 0 occurence of the current character in p, match(s,p,i,j+2)
            // Otherwise return false
            // Note: the mismatch will not be a wildcard as they are taken care of separately
            default:
            return (j+1<p.count && p[j+1] == "*") && match(s,p,i,j+2)
        }
    }
}
```
__O(s\*p) Time, O(s\*p) Space - Bottom-Up Recursion, Top-Down Memoization:__
```Swift
class Solution {
    func isMatch(_ s: String, _ p: String) -> Bool {
        var memo : [[Bool?]] = Array(repeating: Array(repeating: nil, count: p.count), count: s.count)
        return match(Array(s), Array(p), 0, 0,&memo)
    }
    
    func match(_ s: [Character], _ p: [Character], _ i: Int, _ j: Int, _ memo: inout [[Bool?]]) -> Bool {
        // Base case
        switch (i, j) {
            // If s & p both reach the end, then we have a match
            case (s.count, p.count):
            return true
            
            // Process trailing wildcard in p if s reaches the end.
            // s: ""
            // p: "c*c*"
            case (s.count, _):
            return (j+1 < p.count && p[j+1] == "*") && match(s,p,i,j+2,&memo) 
            
            // If p reaches the end before s, it's definitely not a match
            case (_, p.count...Int.max):
            return false
            
            default:
            break
        }

        if let result = memo[i][j] {
            return result
        }
        
        switch p[j] {
            
            // If s[i] matches p[j]: same character or ".", check the following cases:
            case s[i], ".":
            // If the next character in p is a wildcard "*", there are 3 cases to consider:
            // 1. Wildcard represents 0 occurence of p[j], match(s,p,i,j+2)
            // 2. Wildcard represents 1 occurence of p[j], match(s,p,i+1,j+2)
            // 3. Wildcard represents multiple occurences of p[j], match(s,p,i+1,j)
            if (j+1<p.count && p[j+1] == "*") {
                memo[i][j] = match(s,p,i,j+2,&memo) || match(s,p,i+1,j+2,&memo) || match(s,p,i+1,j,&memo)
                break
            }
            // If the next character in p is not a wildcard "*", 
            // continue to match the next characters
            memo[i][j] = match(s,p,i+1,j+1,&memo)
            
            // If the characters do not match:
            // If the next character in p is a wildcard, 
            // it can represent 0 occurence of the current character in p, match(s,p,i,j+2)
            // Otherwise return false
            // Note: the mismatch will not be a wildcard as they are taken care of separately
            default:
            memo[i][j] = (j+1<p.count && p[j+1] == "*") && match(s,p,i,j+2,&memo)
        }
        return memo[i][j]!
    }
}
```