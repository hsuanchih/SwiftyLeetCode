
### First Unique Character in a String

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

__Examples:__
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

__Note:__ You may assume the string contain only lowercase letters.

### Solution
__O(s):__
```Swift
class Solution {
    func firstUniqChar(_ s: String) -> Int {
        let lookup = s.reduce(into: [Character: Int]()) { $0[$1, default:0]+=1 }
        for (index, char) in s.enumerated() {
            if lookup[char] == 1 {
                return index
            }
        }
        return -1
    }
}
```