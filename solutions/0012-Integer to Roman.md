
### Integer to Roman

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

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

__Example 1:__
```
Input: 3
Output: "III"
```
__Example 2:__
```
Input: 4
Output: "IV"
```
__Example 3:__
```
Input: 9
Output: "IX"
```
__Example 4:__
```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```
__Example 5:__
```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

### Solution
```Swift
class Solution {
    let symbolMap : [Int: String] = [
        1000 : "M",
        900 : "CM",
        500 : "D",
        400 : "CD",
        100 : "C",
        90 : "XC",
        50 : "L",
        40 : "XL",
        10 : "X",
        9 : "IX",
        5 : "V",
        4 : "IV",
        1 : "I"
    ]
    
    func intToRoman(_ num: Int) -> String {
        let keys = symbolMap.keys.sorted { $0 > $1 }
        var num = num, result = ""
        while num > 0 {
            for key in keys {
                if num >= key, let roman = symbolMap[key] {
                    result.append(roman)
                    num -= key
                    break
                }
            }
        }
        return result
    }
}
```