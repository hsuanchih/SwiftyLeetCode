
### Multiply Strings

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

__Example 1:__
```
Input: num1 = "2", num2 = "3"
Output: "6"
```
__Example 2:__
```
Input: num1 = "123", num2 = "456"
Output: "56088"
```
__Note:__

The length of both `num1` and `num2` is < 110.
Both `num1` and `num2` contain only digits `0-9`.
Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
You __must not use any built-in BigInteger library__ or __convert the inputs to integer__ directly.

### Solution
__O(n*m) Time, O(n+m) Space:__
```Swift
class Solution {
    func multiply(_ num1: String, _ num2: String) -> String {
        var product : [Int] = Array(repeating: 0, count: num1.count+num2.count)
        let num1 = Array(num1), num2 = Array(num2)
        for j in stride(from: num2.count-1, through: 0, by: -1) {
            for i in stride(from: num1.count-1, through: 0, by: -1) {
                let curr = Int(num2[j].asciiValue!-Character("0").asciiValue!)*Int(num1[i].asciiValue!-Character("0").asciiValue!), 
                temp = product[i+j+1] + curr
                product[i+j+1] = temp%10
                product[i+j] = product[i+j] + temp/10
            }
        }
        let result = product.reduce("") {
            if $0.isEmpty && $1 == 0 {
                return $0
            } else {
                return "\($0)\($1)"
            }
        }
        return result.isEmpty ? "0" : result
    }
}
```