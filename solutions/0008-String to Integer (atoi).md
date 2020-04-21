
### String to Integer (atoi)

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

__Note:__

* Only the space character `' '` is considered as whitespace character.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.

__Example 1:__
```
Input: "42"
Output: 42
```
__Example 2:__
```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```
__Example 3:__
```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```
__Example 4:__
```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```
__Example 5:__
```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.
```

### Solution
__O(str):__
```Swift
class Solution {
    func myAtoi(_ str: String) -> Int {
        let input = Array(str)
        var i : Int = 0, isNegative = false
        while i < input.count && input[i] == " " {
            i+=1
        }
        if i >= input.count || (input[i] != "-" && input[i] != "+" && !input[i].isNumber) {
            return 0
        }
        switch input[i] {
            case "-":
            isNegative = true
            fallthrough
            case "+":
            i+=1
            default:
            break
        }
        var j : Int = i
        while j < input.count && input[j].isNumber  {
            j+=1
        }
        if i == j {
            return 0
        }
        if let result = UInt64(String(input[i..<j])) {
            switch (result, isNegative) {
                case (1<<31 ... UInt64.max, false):
                return (1<<31)-1
                case ((1<<31)+1 ... UInt64.max, true):
                return -(1<<31)
                default:
                return isNegative ? -Int(String(input[i..<j]))! : Int(String(input[i..<j]))!
            }
        }
        return isNegative ? -(1<<31) : (1<<31)-1
    }
}
```
__O(str) - Alternative Solution:__
```Swift
extension Int {
    var converted : Int {
        switch self {
            case (1<<31)...Int.max:
            return (1<<31)-1
            case Int.min..<(-(1<<31)):
            return -(1<<31)
            default:
            return self
        }
    }
}

class Solution {
    func myAtoi(_ str: String) -> Int {
        let str = Array(str)
        
        // Parse each character in str
        // Use i to traverse str
        // Use j to track the state of the parser
        // j == -1: parser hasn't encountered any signs or numbers
        // j gets updated either a sign or number is encountered
        var i = 0, j = -1, sign = 1, result = 0
        while i < str.count {
            switch str[i] {
                
                // Character is a space & j == -1
                // Ignore leading spaces
                case " " where j == -1:
                break
                
                // Character is a sign & j == -1
                // Update the sign of the number,
                // and update j with the index of the sign
                case "-" where j == -1:
                sign = -1
                fallthrough
                case "+" where j == -1:
                j = i
                
                // Character is a numeric digit
                // Update j with the index of the first numeric digit of str
                // Update result calculation & early return if result has reached 
                // 32-bit range
                case let char where char.isNumber:
                if j == -1 || str[j] == "+" || str[j] == "-" {
                    j = i
                }
                result = result*10+Int(String(str[i]))!
                if result >= (1<<31)+1 {
                    return (sign*result).converted
                }
                
                // Character is anything other than a numeric digit
                // ie, a " ", "+", "-", or any other random Character
                default:
                // If we've not yet reached a numeric Character, this is
                // an invalid numeric string, return 0
                // Otherwise, we've reached the end of a valid numeric string
                // return result
                if j == -1 || !str[j].isNumber {
                    return 0
                } else {
                    return (sign*result).converted
                }
            }
            i+=1
        }
        return j == -1 ? 0 : (sign*result).converted
    }
}
```