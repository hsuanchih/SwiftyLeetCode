
### Permutation in String

Given two strings __s1__ and __s2__, write a function to return true if __s2__ contains the permutation of __s1__.</br> 
In other words, one of the first string's permutations is the __substring__ of the second string.

__Example 1:__
```
Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").
```
__Example 2:__
```
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

__Note:__
1. The input strings only contain lower case letters.
2. The length of both given strings is in range [1, 10,000].

### Solution
__O(s2\*26+(s1+s2)) Time, O(26*2) Space:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue!-Character("a").asciiValue!)
    }
}

class Solution {
    func checkInclusion(_ s1: String, _ s2: String) -> Bool {
        let length1 = s1.count, length2 = s2.count
        if length1 > length2 {
            return false
        }
        let s1Map = s1.reduce(into: Array(repeating: 0, count: 26)) { $0[$1.offset]+=1 }, s2 = Array(s2)
        var s2Map = Array(repeating: 0, count: 26)
        for i in 0..<length2 {
            let left = i-length1
            if left >= 0 {
                s2Map[s2[left].offset]-=1
            }
            s2Map[s2[i].offset]+=1
            if s2Map == s1Map {
                return true
            }
        }
        return false
    }
}
```