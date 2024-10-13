
### Add Binary

Given two binary strings `a` and `b`, return their sum as a binary string.

__Example 1:__
```
Input: a = "11", b = "1"
Output: "100"
```
__Example 2:__
```
Input: a = "1010", b = "1011"
Output: "10101"
```

__Constraints:__
* `1 <= a.length, b.length <= pow(10, 4)`
* `a` and `b` consist only of `'0'` or `'1'` characters.
* Each string does not contain leading zeros except for the zero itself.

### Solution
__O(max(a, b)) Time, O(max(a, b)) Space:__
```Swift
class Solution {
    func addBinary(_ a: String, _ b: String) -> String {
        if a.isEmpty {
            return b
        } else if b.isEmpty {
            return a
        } else {
            let a: [Character] = Array(a.reversed())
            let b: [Character] = Array(b.reversed())
            var carry: Character = "0"
            var result: [Character] = []
            let count: Int = max(a.count, b.count)
            for i in 0 ..< count {
                let charA: Character = i < a.count ? a[i] : "0"
                let charB: Character = i < b.count ? b[i] : "0"
                let (val, _carry) = add(charA, charB, carry)
                result.append(val)
                carry = _carry
            }
            if carry == "1" {
                result.append(carry)
            }
            return String(result.reversed())
        }
    }

    func add(_ charA: Character, _ charB: Character, _ carry: Character) -> (Character, Character) {
        switch (charA, charB, carry) {
        case ("0", "0", "0"):
            return ("0", "0")
        case ("1", "0", "0"), 
             ("0", "1", "0"), 
             ("0", "0", "1"):
            return ("1", "0")
        case ("1", "1", "0"), 
             ("1", "0", "1"), 
             ("0", "1", "1"):
            return ("0", "1")
        case ("1", "1", "1"):
            return ("1", "1")
        default:
            fatalError()
        }
    }
}
```