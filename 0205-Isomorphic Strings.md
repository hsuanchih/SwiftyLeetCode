
### Isomorphic Strings

Given two strings __s__ and __t__, determine if they are isomorphic.

Two strings are isomorphic if the characters in __s__ can be replaced to get __t__.

All occurrences of a character must be replaced with another character while preserving the order of characters.</br>
No two characters may map to the same character but a character may map to itself.

__Example 1:__
```
Input: s = "egg", t = "add"
Output: true
```
__Example 2:__
```
Input: s = "foo", t = "bar"
Output: false
```
__Example 3:__
```
Input: s = "paper", t = "title"
Output: true
```
__Note:__
You may assume both s and t have the same length.

### Solution
__O(n):__
```Swift
class Solution {
    class Solution {
    func isIsomorphic(_ s: String, _ t: String) -> Bool {
        let s = Array(s), t = Array(t)
        return isUniqueMapping(s, t) && isUniqueMapping(t, s)
    }
    
    func isUniqueMapping(_ lhs: [Character], _ rhs: [Character]) -> Bool {
        var lookup : [Character: Character] = [:]
        for i in stride(from: 0, to: lhs.count, by: 1) {
            if let char = lookup[lhs[i]] {
                if char != rhs[i] {
                    return false
                }
            } else {
                lookup[lhs[i]] = rhs[i]
            }
        }
        return true
    }
}
```