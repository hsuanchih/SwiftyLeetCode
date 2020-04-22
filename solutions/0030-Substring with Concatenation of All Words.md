
### Substring with Concatenation of All Words

You are given a string, __s__, and a list of words, __words__, that are all of the same length.</br>
Find all starting indices of substring(s) in __s__ that is a concatenation of each word in __words__ exactly once and without any intervening characters.

__Example 1:__
```
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```
__Example 2:__
```
Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []
```

### Solution
__Recursive O(n^2):__
```Swift
class Solution {
    func findSubstring(_ s: String, _ words: [String]) -> [Int] {
        let wordLength = words.first?.count ?? 0
        if s.isEmpty || words.isEmpty || words.first?.isEmpty ?? true || wordLength > s.count {
            return []
        }
        let s = Array(s)
        var lookup = words.reduce(into: [ArraySlice<Character>: Int]()) { $0[ArraySlice($1), default: 0]+=1 }
        var result : [Int] = []
        for i in 0..<s.count where i+wordLength <= s.count {
            if findSubstring(s, i, &lookup, wordLength) {
                result.append(i)
            }
        }
        return result
    }
    
    func findSubstring(_ s: [Character], _ index: Int, _ lookup: inout [ArraySlice<Character>: Int], _ wordLength: Int) -> Bool {
        if lookup.isEmpty {
            return true
        }
        if index+wordLength > s.count {
            return false
        }
        let word = s[index..<index+wordLength]
        if let count = lookup[word] {
            if count == 1 {
                lookup.removeValue(forKey: word)
            } else {
                lookup[word] = count-1
            }
            let result = findSubstring(s, index+wordLength, &lookup, wordLength)
            lookup[word, default:0]+=1
            return result
        }
        return false
    }
}
```
__Iterative O(n^2):__
```Swift
class Solution {
    func findSubstring(_ s: String, _ words: [String]) -> [Int] {
        let wordLength = words.first?.count ?? 0
        if s.isEmpty || words.isEmpty || words.first?.isEmpty ?? true || wordLength > s.count {
            return []
        }
        let s = Array(s), lookup = words.reduce(into: [ArraySlice<Character>: Int]()) { $0[ArraySlice($1), default: 0]+=1 }
        var result : [Int] = []
        for i in 0..<s.count where i+wordLength*words.count <= s.count {
            if findSubstring(s, i, wordLength, words.count, lookup) {
                result.append(i)
            }
        }
        return result
    }
    
    func findSubstring(_ s: [Character], _ index: Int, _ wordLength: Int, _ wordsCount: Int, _ lookup: [ArraySlice<Character>: Int]) -> Bool {
        var lookup = lookup
        for i in stride(from: index, through: index+wordLength*(wordsCount-1), by: wordLength) {
            let word = s[i..<i+wordLength]
            if let count = lookup[word] {
                if count == 1 {
                    lookup.removeValue(forKey: word)
                } else {
                    lookup[word] = count-1
                }
            } else {
                return false
            }
        }
        return lookup.isEmpty
    }
}
```