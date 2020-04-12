
### Count and Say

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
For example, two is written as `II` in Roman numeral, just two one's added together.</br> 
Twelve is written as, `XII`, which is simply `X` + `II`.</br> 
The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right.</br> 
However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`.</br>
Because the one is before the five we subtract it making four.</br>
The same principle applies to the number nine, which is written as `IX`. 
There are six instances where subtraction is used:

* `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
* `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
* `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

__Example 1:__
```
Input: "III"
Output: 3
```
__Example 2:__
```
Input: "IV"
Output: 4
```
__Example 3:__
```
Input: "IX"
Output: 9
```
__Example 4:__
```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```
__Example 5:__
```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

### Solution
__O(s) Time, O(1) Space:__
```Swift
class Solution {
    
    let symbolMap : [Character: Int] = [
        "I" : 1,
        "V" : 5,
        "X" : 10,
        "L" : 50,
        "C" : 100,
        "D" : 500,
        "M" : 1000
    ]
    
    func romanToInt(_ s: String) -> Int {
        var result = 0
        let input = Array(s)
        for i in stride(from: 0, to: input.count, by: 1) {
            let char = input[i], curr = symbolMap[char]!
            result += curr
            if i > 0 {
                let prevChar = input[i-1], prev = symbolMap[prevChar]!
                if prev < curr {
                    result -= 2*prev
                }
            }
        }
        return result
    }
}
```