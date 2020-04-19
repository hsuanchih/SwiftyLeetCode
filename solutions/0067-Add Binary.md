
### Add Binary

Given two binary strings, return their sum (also a binary string).

The input strings are both __non-empty__ and contains only characters `1` or `0`.

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

### Solution
__O(n):__
```Swift
class Solution {
    func addBinary(_ a: String, _ b: String) -> String {
        var result : String = "", carry : Int = 0
        
        let a : [Character] = a.reversed(), 
        b : [Character] = b.reversed(), 
        length = max(a.count, b.count)
        
        for i in stride(from: 0, to: length, by: 1) {
            let num1 = (i < a.count ? a[i] : "0"), 
            num2 = (i < b.count ? b[i] : "0")
            switch (num1, num2) {
                case ("0", "0"):
                result.append(carry == 1 ? "1" : "0")
                carry = 0
                case ("1", "1"):
                result.append(carry == 1 ? "1" : "0")
                carry = 1
                default:
                result.append(carry == 1 ? "0" : "1")
            }
        }
        if carry == 1 {
            result.append("1")
        }
        return String(result.reversed())
    }
}
```