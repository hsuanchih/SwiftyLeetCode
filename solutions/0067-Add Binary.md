
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