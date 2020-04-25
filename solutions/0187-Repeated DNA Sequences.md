
### Repeated DNA Sequences

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

__Example:__
```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

Output: ["AAAAACCCCC", "CCCCCAAAAA"]
```

### Solution
__O(10*s) Time - HashMap:__
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
__O(s+10*result) Time - HashMap + Sliding-Window Bit-Manipulation:__
```Swift
extension Character {
    var dnaValue : Int {
        switch self {
            case "A": return 0
            case "C": return 1
            case "G": return 2
            case "T": return 3
            default: fatalError()
        }
    }
}

extension Int {
    var dnaChar : Character {
        switch self {
            case 0: return "A"
            case 1: return "C"
            case 2: return "G"
            case 3: return "T"
            default: fatalError()
        }
    }
}

class Solution {
    func findRepeatedDnaSequences(_ s: String) -> [String] {
        let s = Array(s)
        var lookup : [Int: Int] = [:], curr = 0
        for i in 0..<s.count {
            curr = (curr << 2) & 0xfffff | s[i].dnaValue
            if i >= 9 {
                lookup[curr, default: 0]+=1
            }
        }
        var result : [String] = []
        for (key, value) in lookup {
            if value > 1 {
                var temp = ""
                for bit in stride(from: 9, through: 0, by: -1) {
                    let char = ((key >> (bit*2)) & 3).dnaChar
                    temp.append(char)
                }
                result.append(temp)
            }
        }
        return result
    }
}
```