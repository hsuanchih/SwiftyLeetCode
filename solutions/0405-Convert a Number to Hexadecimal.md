
### Convert a Number to Hexadecimal

Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

__Note:__
1. All letters in hexadecimal (a-f) must be in lowercase.
2. The hexadecimal string must not contain extra leading `0`s. If the number is zero, it is represented by a single zero character `'0'`; otherwise, the first character in the hexadecimal string will not be the zero character.
3. The given number is guaranteed to fit within the range of a `32-bit signed integer`.
4. You must not use any method provided by the library which converts/formats the number to hex directly.

__Example 1:__
```
Input:
26

Output:
"1a"
```
__Example 2:__
```
Input:
-1

Output:
"ffffffff"
```

### Solution
__O(num) Time, O(1) Space - Recursive:__
```Swift
class Solution {
    func toHex(_ num: Int) -> String {
        switch num {
            
            // If num is negative, convert num to its 2's compliment
            case Int.min..<0:
            return toHex(1<<32-abs(num))
            
            // Base case, convert num to base 16 string
            case 0...15:
            return base16(num)
            
            // num >= 16, recurse the computation and append num%16 to result
            default:
            return toHex(num/16) + base16(num%16)
        }
    }
    
    // Helper method to convert Int in 0...15 to its base 16 string representation
    func base16(_ num: Int) -> String {
        switch num {
            case 0...9:
            return String(num)
            default:
            return String(Character(UnicodeScalar(UInt8(num-10)+Character("a").asciiValue!)))
        }
    }
}
```