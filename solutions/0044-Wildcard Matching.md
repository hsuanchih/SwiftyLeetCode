
### Wildcard Matching

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'`.
```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```
The matching should cover the __entire__ input string (not partial).

__Note:__
* `s` could be empty and contains only lowercase letters `a-z`.
* `p` could be empty and contains only lowercase letters `a-z`, and characters like `?` or `*`.

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
p = "*"
Output: true
Explanation: '*' matches any sequence.
```
__Example 3:__
```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```
__Example 4:__
```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```
__Example 5:__
```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

### Solution
__O(3^max(s,p)+(s+p)) Time, O(1) Space - Bottom-Up Recursive:__
```Swift
class Solution {
    func isMatch(_ s: String, _ p: String) -> Bool {
        return match(Array(s),Array(p),0,0)
    }
    
    func match(_ s: [Character], _ p: [Character], _ i: Int, _ j: Int) -> Bool {
        
        // Base case
        switch (i, j) {
            
            // If i & j reach the end of s & p, 
            // it's a match
            case (s.count, p.count):
            return true
            
            // If j reaches the end of p before i reaches the end of s,
            // it's not a match
            case (_, p.count):
            return false
            
            // If i reaches the end of s, we need to check the rest of p:
            // p[j] == "*" can be treated as an empty character, continue
            // checking the rest of p
            case (s.count, _):
            return p[j] == "*" && match(s,p,i,j+1)
            
            default:
            break
        }
        switch p[j] {
            
            // If s[i] matches p[j] or if p[j] == "?",
            // the current characters in s & p are a match
            // match the next characters
            case s[i], "?":
            return match(s,p,i+1,j+1)
            
            // If p[j] == "*", it can represent either of the 3 cases
            // "*" matches 0 character: match(s,p,i,j+1)
            // "*" matches 1 character: match(s,p,i+1,j+1)
            // "*" matches multiple characters: match(s,p,i+1,j)
            case "*":
            return match(s,p,i+1,j+1) || match(s,p,i,j+1) || match(s,p,i+1,j)
            
            // If the characters don't macth, return false
            default:
            return false
        }
    }
}
```
__O((s\*p)+(s+p)) Time, O(s\*p) Space - Bottom-Up Recursive, Top-Down Memoization:__
```Swift
class Solution {
    func isMatch(_ s: String, _ p: String) -> Bool {
        var memo : [[Bool?]] = Array(repeating: Array(repeating: nil, count: p.count), count: s.count)
        return match(Array(s),Array(p),0,0,&memo)
    }
    
    func match(_ s: [Character], _ p: [Character], _ i: Int, _ j: Int, _ memo: inout [[Bool?]]) -> Bool {
        switch (i, j) {
            case (s.count, p.count):
            return true
            case (_, p.count):
            return false
            case (s.count, _):
            return p[j] == "*" && match(s,p,i,j+1,&memo)
            default:
            break
        }
        
        if let result = memo[i][j] {
            return result
        }
        
        switch p[j] {
            case s[i], "?":
            memo[i][j] = match(s,p,i+1,j+1,&memo)
            case "*":
            memo[i][j] = match(s,p,i+1,j+1,&memo) || match(s,p,i,j+1,&memo) || match(s,p,i+1,j,&memo)
            default:
            memo[i][j] = false
        }
        return memo[i][j]!
    }
}
```