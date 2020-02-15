
### Repeated DNA Sequences

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

__Example:__
```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

Output: ["AAAAACCCCC", "CCCCCAAAAA"]
```

### Solution
__O(10*n):__
```Swift
class Solution {
    func findRepeatedDnaSequences(_ s: String) -> [String] {
        let s = Array(s)
        var map = [ArraySlice<Character>: Int]()
        for i in 0..<s.count where i + 10 <= s.count {
            map[s[i..<i+10], default: 0] += 1
        }
        return map.lazy.filter { $0.value > 1 }.map { String($0.key) }
    }
}
```