
### Integer to Roman

Seven different symbols represent Roman numerals with the following values:

| Symbol | Value |
| ------ | ----- |
| I | 1    |
| V | 5    |
| X	| 10   |
| L	| 50   |
| C	| 100  |
| D	| 500  |
| M	| 1000 |

Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:
* If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.
* If the value starts with 4 or 9 use the subtractive form representing one symbol subtracted from the following symbol, for example, 4 is 1 (`I`) less than 5 (`V`): `IV` and 9 is 1 (`I`) less than 10 (`X`): `IX`. Only the following subtractive forms are used: 4 (`IV`), 9 (`IX`), 40 (`XL`), 90 (`XC`), 400 (`CD`) and 900 (`CM`).
* Only powers of 10 (`I`, `X`, `C`, `M`) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (`V`), 50 (`L`), or 500 (`D`) multiple times. If you need to append a symbol 4 times use the subtractive form.

Given an integer, convert it to a Roman numeral.

__Example 1:__
```
Input: num = 3749

Output: "MMMDCCXLIX"

Explanation:

3000 = MMM as 1000 (M) + 1000 (M) + 1000 (M)
 700 = DCC as 500 (D) + 100 (C) + 100 (C)
  40 = XL as 10 (X) less of 50 (L)
   9 = IX as 1 (I) less of 10 (X)
Note: 49 is not 1 (I) less of 50 (L) because the conversion is based on decimal places
```
__Example 2:__
```
Input: num = 58

Output: "LVIII"

Explanation:

50 = L
 8 = VIII
Example 3:

Input: num = 1994

Output: "MCMXCIV"

Explanation:

1000 = M
 900 = CM
  90 = XC
   4 = IV
``` 

__Constraints:__
* `1 <= num <= 3999`

### Solution
__Solution 1:__
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
__Solution 2:__
```Swift
class Solution {
    func intToRoman(_ num: Int) -> String {
        var num: Int = num
        var trailingZeros: Int = 0
        var result: String = ""
        while num > 0 {
            let digit: Int = num % 10
            result = symbol(for: digit, trailingZeros: trailingZeros) + result
            num /= 10
            trailingZeros += 1
        }
        return result
    }

    func symbol(for digit: Int, trailingZeros: Int) -> String {
        switch (digit, trailingZeros) {
        case (0, _):
            return ""
        case (1 ... 3, 0):
            return String(Array(repeating: "I", count: digit))
        case (4, 0):
            return "IV"
        case (5, 0):
            return "V"
        case (6 ... 8, 0):
            return "V" + String(Array(repeating: "I", count: digit - 5))
        case (9, 0):
            return "IX"
        case (1 ... 3, 1):
            return String(Array(repeating: "X", count: digit))
        case (4, 1):
            return "XL"
        case (5, 1):
            return "L"
        case (6 ... 8, 1):
            return "L" + String(Array(repeating: "X", count: digit - 5))
        case (9, 1):
            return "XC"
        case (1 ... 3, 2):
            return String(Array(repeating: "C", count: digit))
        case (4, 2):
            return "CD"
        case (5, 2):
            return "D"
        case (6 ... 8, 2):
            return "D" + String(Array(repeating: "C", count: digit - 5))
        case (9, 2):
            return "CM"
        case (1 ... 3, 3):
            return String(Array(repeating: "M", count: digit))
        default:
            fatalError()
        }
    }
}
```