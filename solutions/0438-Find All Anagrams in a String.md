
### Find All Anagrams in a String

Given a string __s__ and a __non-empty__ string __p__, find all the start indices of __p__'s anagrams in __s__.

Strings consists of lowercase English letters only and the length of both strings __s__ and __p__ will not be larger than 20,100.

The order of output does not matter.

__Example 1:__
```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```
__Example 2:__
```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

### Solution
__O((26+p)\*s+(s+p)) Time, O(26*2) Space:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue!-Character("a").asciiValue!)
    }
}

class Solution {
    func findAnagrams(_ s: String, _ p: String) -> [Int] {
        if s.count < p.count {
            return []
        }
        let pMap = p.reduce(into: Array(repeating: 0, count: 26)) { $0[$1.offset]+=1 }, s = Array(s)
        var result : [Int] = []
        for start in 0...s.count-p.count {
            var sMap : [Int] = Array(repeating: 0, count: 26)
            for end in 0..<p.count {
                sMap[s[start+end].offset]+=1
            }
            if sMap == pMap {
                result.append(start)
            }
        }
        return result
    }
}
```
__O((26)\*s+(s+p)) Time, O(26*2) Space:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue!-Character("a").asciiValue!)
    }
}

class Solution {
    func findAnagrams(_ s: String, _ p: String) -> [Int] {
        let pMap = p.reduce(into: Array(repeating: 0, count: 26)) { $0[$1.offset]+=1 }, s = Array(s)
        var sMap : [Int] = Array(repeating: 0, count: 26), result : [Int] = []
        
        // Keep a sliding window of length p and update sMap
        for i in 0..<s.count {
            sMap[s[i].offset]+=1
            let left = i-p.count
            if left >= 0 {
                sMap[s[left].offset]-=1
            }
            if sMap == pMap {
                result.append(left+1)
            }
        }
        return result
    }
}
```