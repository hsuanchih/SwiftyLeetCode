
### Scramble String

Given a string __s1__, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of __s1__ = `"great"`:
```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```
To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node `"gr"` and swap its two children, it produces a scrambled string `"rgeat"`.
```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```
We say that `"rgeat"` is a scrambled string of `"great"`.

Similarly, if we continue to swap the children of nodes `"eat"` and `"at"`, it produces a scrambled string `"rgtae"`.
```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```
We say that `"rgtae"` is a scrambled string of `"great"`.

Given two strings __s1__ and __s2__ of the same length, determine if __s2__ is a scrambled string of __s1__.

__Example 1:__
```
Input: s1 = "great", s2 = "rgeat"
Output: true
```
__Example 2:__
```
Input: s1 = "abcde", s2 = "caebd"
Output: false
```

### Solution
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue! - Character("a").asciiValue!)
    }
}

extension Array where Element == Character {
    func tally() -> [Int] {
        return reduce(into: Array<Int>(repeating: 0, count: 26)) {
            $0[$1.offset]+=1
        }
    }
}

class Solution {
    func isScramble(_ s1: String, _ s2: String) -> Bool {
        return solve(Array(s1), Array(s2))
    }
    
    func solve(_ s1: [Character], _ s2: [Character]) -> Bool {
        if s1 == s2 {
            return true
        }
        
        guard s1.tally() == s2.tally() else {
            return false
        }
        
        for i in stride(from: 1, to: s1.count, by: 1) {
            if solve(Array(s1[0..<i]), Array(s2[0..<i])) && solve(Array(s1[i..<s1.count]), Array(s2[i..<s1.count])) {
                return true
            }
            if solve(Array(s1[0..<i]), Array(s2[s1.count-i..<s1.count])) && solve(Array(s1[i..<s1.count]), Array(s2[0..<s1.count-i])) {
                return true
            }
        }
        return false
    }
}
```